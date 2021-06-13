<template>
  <div>
    <ul>
      <li v-for="product in products.edges" :key="product.node.id">
        <ProductCard :product="product.node" />
      </li>
    </ul>
  </div>
</template>

<script>
import getAllProducts from "~/graphql/products"

export default {
  apollo: {
    products: {
      query: getAllProducts
    }
  }
}
</script>
