<template>
  <div>
    <CardList>
      <ProductCard
        v-for="product in products.edges"
        :key="product.node.id"
        :product="product.node"
      />
    </CardList>
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
