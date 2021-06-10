<template>
  <div class="flex flex-wrap">
    <div class="w-full md:w-1/2">
      <img
        v-for="image in product.images"
        :key="image.id"
        :src="image.src"
        alt=""
      />
    </div>
    <div class="w-full md:w-1/2">
      <h1>{{ product.title }}</h1>
      <form v-if="product.options" action="" method="post">
        <div v-for="option in product.options" :key="option.id">
          <label :for="'options_' + option.name">{{ option.name }}</label>
          <select
            @change="optionSelect($event, option.name)"
            name="options"
            :id="'options_' + option.name"
          >
            <option value="">--Select--</option>
            <option
              v-for="value in option.values"
              :key="value.index"
              :value="value.value"
            >
              {{ value.value }}
            </option>
          </select>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      handle: this.$route.params.handle,
      selectedOptions: [],
      selectedVariant: ''
    }
  },
  methods: {
    optionSelect(e, optionName) {
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

      this.calculateVariant(this.selectedOptions)
      this.variantBySelectedOptions(this.selectedOptions)
    },
    calculateVariant(options) {
      console.log('calculateVariant', options)
    },
    async variantBySelectedOptions(options) {
      console.log(this.$shopify)
      // this.variant = await this.$shopify.product.variantBySelectedOptions(
      //   options
      // )
    }
  },
  async asyncData({ $shopify, params }) {
    const product = await $shopify.product.fetchByHandle(params.handle)
    const options = product.options
    const variants = product.variants
    return { product, options, variants }
  }
}
</script>
