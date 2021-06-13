<template>
  <header class="sticky top-0 w-full px-4 py-6 bg-white dark:bg-gray-900">
    <div class="container flex justify-between">
      <div class="w-full lg:w-1/3">
        <a class="text-gray-900 dark:text-gray-100" href="/"
          >Mallin Skate Supplies</a
        >
      </div>
      <nav class="w-full lg:w-1/2">
        <ul class="flex justify-end">
          <li class="px-2">
            <a class="text-gray-900 dark:text-gray-100" href="/shoes">Shoes</a>
          </li>
          <li class="px-2">
            <a class="text-gray-900 dark:text-gray-100" href="/decks">Decks</a>
          </li>
        </ul>
      </nav>
    </div>
  </header>
</template>

<script>
export default {
  props: {
    activePage: {
      type: String,
      default: "/"
    }
  }
}
</script>
