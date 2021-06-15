<template>
  <div>
    <p>カテゴリ一覽</p>
    <Categories :categories="categories" />
    <p>記事一覽</p>
    <Articles :contents="contents" />
    <p>ページ送り</p>
    <Pager :max="pagerMax" :category="categoryId"></Pager>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  // paramsはURLにnuxt.config.jsでrouterに指定したパラメーターが入る
  // このサンプルではnuxt.config.jsのrouter :p と :categoryId の2つのパラメーターを指定している
  async asyncData({params}) {
    const page = params.p || '1'
    const categoryId = params.categoryId
    const limit = 5
    console.log(page)
    console.log(categoryId)

    // カテゴリ一覽用
    const categories = await axios.get(
      `https://cdl2021.microcms.io/api/v1/categories?limit=100`,
      {
        headers: { 'X-API-KEY': 'a6bae4eb-c7cb-498f-b87c-426eb33c9f7b' }
      }
    )

    // 記事一覽用
    // ``はテンプレート構文。文字とスクリプトを混ぜ込んで書くことができる
    // ``の中でスクリプトを書くときは${}のカッコの中にスクリプトを入れる
    // ${}の中にさらに``を入れることも可能
    // ${}の中の``の中に${}を入れることも可能（入れ子ができるということ）
    // `aaaaa ${ categoryId === undefined : '' : `${}` }`
    const { data } = await axios.get(
      `https://cdl2021.microcms.io/api/v1/blog?limit=${limit}${
        categoryId === undefined ? '' : `&filters=category[equals]${categoryId}`
      }&offset=${(page - 1) * limit}`,
      {
        headers: { 'X-API-KEY': 'a6bae4eb-c7cb-498f-b87c-426eb33c9f7b' }
      }
    )
    // ...dataはスプレッド構文で、オブジェクト内のプロパティを展開します
    // data: { firstName: 'Fuminori', lastName: 'Mori' }というオブジェクトがあったとして、
    // return { ...data }をすると受取先では firstName と lastName を直接指定できます
    // → data.firstName,data.lastNameとしなくてよい
    // 通信で持ってくるデータの形式がmicroCMSだとどれもcontentsに入ってくるため、APIごとに呼び出し方を調整するためにスプレッド構文を使います

    // Mathは数に関するいろんな関数を持つオブジェクト
    // Math.ceilは引数に指定した数値から見て最小の整数（2.2を指定すれば3が返る）
    // つまり、下記のようにページ送りの数を指定できる
    // 11/5=2.2(割ったあまりがある)→ 3ページの一覽がある
    // 10/5=2(割ったあまりがない)→ 2ページの一覽がある
    return {
      ...data, // 記事データのcontents, limit, offset, totalCountを直接使えるようにする
      page,
      categoryId,
      pagerMax: Math.ceil(data.totalCount / limit),
      categories: categories.data.contents
    }
  },
}
</script>

<style lang="scss" scoped>
a {
  text-decoration: none;
}
</style>
