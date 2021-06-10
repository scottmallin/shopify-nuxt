<template>
  <div>
    {{ shop }}
    {{ products }}
    <ul class="pt-4">
      <li v-for="product in products.edges" :key="product.id">
        {{ product.title }}
      </li>
      <!-- <li v-for="product in products.edges" :key="product.id">
        <ProductCard :product="product" />
      </li> -->
    </ul>
  </div>
</template>

<script>
import getAllProducts from '~/graphql/products'
import getShopName from '~/graphql/shop'

export default {
  apollo: {
    shop: {
      query: getShopName,
      update: (data) => data.shop
    },
    products: {
      query: getAllProducts,
      update: (data) => data.products
    }
  },
  data() {
    return {
      shop: '',
      products: ''
    }
  }
}
</script>
