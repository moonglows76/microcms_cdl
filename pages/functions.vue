<template>
 <div class="container">
  <h2 v-text="'Netlify Functions × Nuxt.js サンプル'" />
  <!-- netlify functionsから受け取ったものを表示 -->
  <p v-text="message" />
 </div>
</template>

<script>
// axiosの読み込み
import axios from 'axios'
export default {
 asyncData() {
  const baseUrl =
   process.env.NODE_ENV !== 'production'
    ? 'http://localhost:9000'
    : process.env.URL
  return axios.get(baseUrl + '/.netlify/functions/hello').then((response) => {
    return {
      message: response.data
    }
  })
 }
}
</script>
