<template>
  <ul class="pager">
    <li v-for="page in max" :key="page.id">
      <nuxt-link :to="`${category === undefined ? '' : `/category/${category}`
      }/page/${page}`">
        {{ page }}
      </nuxt-link>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    max: { type: Number },
    category: { type: String },
  },
}
</script>

<style>

</style>
