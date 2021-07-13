<template>
  <div class="wrapper">
    <div class="divider">
      <div class="container">
        <!-- 検索テキストボックス -->
        <!-- 入力中にenterキーを押したらmethods内にあるsearch関数を呼び出す -->
        <!-- 何か入力したら検索可能のフラグをたてる -->
        <input
          v-model="q"
          class="search"
          type="text"
          @keyup.enter="(e) => search(e.target.value)"
          @keypress="setSearchable"
        />

        <!-- ローディング表示 -->
        <div v-if="loading === true" class="loader">
          検索中・・・
        </div>
        <div v-else>

          <!-- 検索結果が0件時の表示 -->
          <!-- v-showで表示する条件を指定している -->
          <div
            v-show="loading === false && contents.length === 0"
            class="loader"
          >
            記事がありません
          </div>

          <!-- 検索結果表示 -->
          <!-- 検索結果が0件のときもul要素は表示されるがli要素はないし、 -->
          <ul>
            <li v-for="content in contents" :key="content.id" class="list">
              <nuxt-link :to="`/${content.id}`" class="link">
                {{ content.title }}
              </nuxt-link>
            </li>
          </ul>

        </div>
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  // データを格納する器の宣言
  data() {
    return {
      searchable: true,
      contents: this.contents || [],
      loading: true,
      q: this.$route.query.q,
    };
  },
  created() {
    // URLに含まれている検索文言を変数queryに入れて、methods内のsearch関数に渡す
    const query = this.$route.query;
    this.search(query.q);
  },
  methods: {
    // 検索可能かどうかのフラグ
    setSearchable() {
      this.searchable = true;
    },
    // 検索する
    async search(q = '') {
      if (!q.trim() || !this.searchable) {
        return;
      }
      // ローディング表示開始
      this.loadingStart();

      console.log(`q: ${q}`)

      // netlify functionsの呼び出し
      const { data, error } = await axios
        .get(`/.netlify/functions/search?q=${q}`)
        .catch((error) => ({ error }));

      // ローディング表示終了
      this.loadingFinish();

      // 通信でエラーが出ていたら
      if (error) {
        return;
      }

      // 取得したデータの格納
      this.contents = data.contents;
      this.searchable = false;
    },
    // ローディング表示
    loadingStart() {
      this.loading = true;
    },
    // ローディング非表示
    loadingFinish() {
      this.loading = false;
    },
  },
};
</script>
