<template>
  <ul>
    <li v-for="content in contents" :key="content.id">
      <nuxt-link :to="`/${content.id}`">
        {{ content.title }}<br>
        カテゴリ：{{ content.category.name }}
      </nuxt-link>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    contents: { type: Array },
  },
}
</script>

<style>

</style>
