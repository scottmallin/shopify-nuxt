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
import gql from 'graphql-tag'

import getAllProducts from '~/graphql/products'

export default {
  apollo: {
    shop: gql`
      {
        shop {
          name
        }
      }
    `,
    products: {
      query: getAllProducts
    }
  }
}
</script>
