<template>
  <div class="flex flex-wrap">
    <div class="w-full md:w-1/2">
      <img
        v-for="image in product.images.edges"
        :key="image.node.index"
        :src="image.node.originalSrc"
        alt=""
      />
    </div>
    <div class="w-full md:w-1/2">
      <h1>{{ product.title }}</h1>
      <form v-if="product.options" action="" method="post">
        <div v-for="option in product.options" :key="option.id">
          <label :for="'options_' + option.name">{{ option.name }}</label>
          <select
            @change="findVariant"
            name="options"
            :id="'options_' + option.name"
          >
            <option value="">--Select--</option>
            <option
              v-for="value in option.values"
              :key="value.index"
              :value="value"
            >
              {{ value }}
            </option>
          </select>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import productByHandle from "~/graphql/productByHandle"
import getVariant from "~/graphql/getVariant"

export default {
  data() {
    return {
      handle: this.$route.params.handle,
      selectedOptions: [],
      selectedVariant: ""
    }
  },
  async asyncData({ app, params }) {
    const client = app.apolloProvider.defaultClient
    const { handle } = params

    const res = await client.query({
      query: productByHandle,
      variables: {
        handle: handle
      }
    })

    const product = res.data.productByHandle
    return {
      product
    }
  },
  methods: {
    onOptionSelect(e) {
      console.log("onOptionSelect", e)
    },
    async findVariant() {
      const productID = this.product.id

      const client = this.$apollo.getClient()
      const res = await client.query({
        query: getVariant,
        variables: {
          id: productID,
          name: "Color",
          value: "Green"
        }
      })

      this.selectedVariant = res
    }
  }
}
</script>
