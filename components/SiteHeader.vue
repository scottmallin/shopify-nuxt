<template>
  <header class="sticky top-0 w-full px-4 py-6 bg-white">
    <a href="/">Mallin Skate Supplies</a>
  </header>
</template>

<script>
export default {
  props: {
    activePage: {
      type: String,
      default: '/',
    },
  },
}
</script>
