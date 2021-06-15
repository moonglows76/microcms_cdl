<template>
  <ul class="category-list">
    <li>
      <nuxt-link :to="`/`">
        ALL
      </nuxt-link>
    </li>
    <li v-for="category in categories" :key="category.id">
      <nuxt-link :to="`/category/${category.id}/page/1`">
        {{ category.name }}
      </nuxt-link>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    categories: {
      type: Object
    }
  }
}
</script>

<style>

</style>
