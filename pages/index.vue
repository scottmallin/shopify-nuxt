<template>
  <div>
    <ul class="pt-4">
      <li v-for="product in products" :key="product.id">
        <ProductCard :product="product" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $shopify, params }) {
    const products = await $shopify.product.fetchAll()
    console.log(products)
    return { products }
  },
}
</script>
