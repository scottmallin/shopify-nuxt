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
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      handle: this.$route.params.handle,
    }
  },
  async asyncData({ $shopify, params }) {
    const product = await $shopify.product.fetchByHandle(params.handle)
    console.log(product.variants)
    return { product }
  },
}
</script>
