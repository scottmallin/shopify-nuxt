<template>
  <div class="container">
    <div class="flex flex-wrap -mx-2">
      <div class="w-full md:w-1/2 px-2">
        <img
          v-for="image in product.images.edges"
          :key="image.node.index"
          :src="image.node.originalSrc"
          alt=""
        />
      </div>
      <div class="w-full md:w-1/2 px-2 flex flex-wrap items-center">
        <div class="self-start">
          <button @click="$router.go(-1)">Back</button>
        </div>
        <div class="self-start w-full">
          <h1 class="text-4xl mb-2">{{ product.title }}</h1>
          <h2
            class="text-2xl mb-6"
            v-if="
              !selectedVariant ||
              selectedVariant.data.node.variantBySelectedOptions === null
            "
          >
            From {{ product.priceRange.minVariantPrice.amount }}
          </h2>
          <h2 class="text-2xl mb-6" v-else>
            {{
              selectedVariant.data.node.variantBySelectedOptions.priceV2.amount
            }}
          </h2>
          <form v-if="product.options" action="" method="post">
            <div v-for="option in product.options" :key="option.id">
              <label :for="'options_' + option.name">{{ option.name }}</label>
              <select
                class="block w-full border p-2 rounded"
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
