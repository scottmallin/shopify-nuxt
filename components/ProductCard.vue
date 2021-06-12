<template>
  <div class="border border-gray-300 bg-white p-4">
    <a :href="'/products/' + product.handle">
      <img
        :src="product.images.edges[0].node.originalSrc"
        :alt="product.title"
        class="mx-auto mb-4"
        loading="lazy"
      />
      <h3 class="font-semibold text-base leading-tight">{{ product.title }}</h3>
      <div class="pt-4 text-sm text-gray-700">{{ product.description }}</div>
    </a>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
      default: null
    }
  }
}
</script>
