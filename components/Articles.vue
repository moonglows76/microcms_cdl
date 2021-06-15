<template>
  <ul>
    <li v-for="content in contents" :key="content.id">
      <nuxt-link :to="`/${content.id}`" class="p-6 my-6 bg-white rounded-xl shadow-md flex items-center space-x-4">
        {{ content.title }}<br>
        カテゴリ：{{ content.category.name }}
      </nuxt-link>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    contents: { type: Object },
  },
}
</script>

<style>

</style>
