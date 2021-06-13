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
      <h2
        v-if="
          !selectedVariant ||
          selectedVariant.data.node.variantBySelectedOptions === null
        "
      >
        From {{ product.priceRange.minVariantPrice.amount }}
      </h2>
      <h2 v-else>
        {{ selectedVariant.data.node.variantBySelectedOptions.priceV2.amount }}
      </h2>
      <form v-if="product.options" action="" method="post">
        <div v-for="option in product.options" :key="option.id">
          <label :for="'options_' + option.name">{{ option.name }}</label>
          <select
            @change="onOptionSelect($event, option.name)"
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
import basketAdd from "~/graphql/basket"

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
    onOptionSelect(e, optionName) {
      const option = {
        name: optionName,
        value: e.target.value
      }

      if (this.selectedOptions.findIndex((f) => f.name === optionName) != -1) {
        this.$set(
          this.selectedOptions,
          this.selectedOptions.findIndex((f) => f.name === optionName),
          option
        )
      } else {
        this.selectedOptions.push(option)
      }

      this.findVariant(this.selectedOptions)
    },
    findVariant(options) {
      const productID = this.product.id
      const client = this.$apollo.getClient()

      client
        .query({
          query: getVariant,
          variables: {
            id: productID,
            options: options
            // name: "Color",
            // value: "Green"
          }
        })
        .then((res) => {
          this.selectedVariant = res
        })
    }
    // onBasketAdd() {
    //   const client = this.$apollo.getClient()

    //   client.mutate({
    //     mutation: basketAdd,
    //     variables: {
    //       checkoutId:
    //     }
    //   })
    // }
  }
}
</script>
