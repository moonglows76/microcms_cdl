<template>
  <main class="main">

    <!-- <button type="button" onclick="doGTranslate('ja|zh-CN');return false;">中国語</button>
    <button type="button" onclick="doGTranslate('ja|en');return false;">英語</button>
    <button type="button" onclick="doGTranslate('ja|ja');return false;">日本語</button>
    <button type="button" onclick="doGTranslate('ja|ko');return false;">韓国語</button>

    <div id="google_translate_element2"></div>
    <script type="text/javascript">
    function googleTranslateElementInit2() {new google.translate.TranslateElement({pageLanguage: 'ja',autoDisplay: false}, 'google_translate_element2');}
    </script><script type="text/javascript" src="https://translate.google.com/translate_a/element.js?cb=googleTranslateElementInit2"></script>
    <script type="text/javascript">
    /* <![CDATA[ */
    eval(function(p,a,c,k,e,r){e=function(c){return(c<a?'':e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)r[e(c)]=k[c]||e(c);k=[function(e){return r[e]}];e=function(){return'\\w+'};c=1};while(c--)if(k[c])p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return p}('6 7(a,b){n{4(2.9){3 c=2.9("o");c.p(b,f,f);a.q(c)}g{3 c=2.r();a.s(\'t\'+b,c)}}u(e){}}6 h(a){4(a.8)a=a.8;4(a==\'\')v;3 b=a.w(\'|\')[1];3 c;3 d=2.x(\'y\');z(3 i=0;i<d.5;i++)4(d[i].A==\'B-C-D\')c=d[i];4(2.j(\'k\')==E||2.j(\'k\').l.5==0||c.5==0||c.l.5==0){F(6(){h(a)},G)}g{c.8=b;7(c,\'m\');7(c,\'m\')}}',43,43,'||document|var|if|length|function|GTranslateFireEvent|value|createEvent||||||true|else|doGTranslate||getElementById|google_translate_element2|innerHTML|change|try|HTMLEvents|initEvent|dispatchEvent|createEventObject|fireEvent|on|catch|return|split|getElementsByTagName|select|for|className|goog|te|combo|null|setTimeout|500'.split('|'),0,{}))
    /* ]]> */
    </script>

    東京 -->

    <p>カテゴリ一覽</p>
    <Categories :categories="categories" />
    <p>記事一覽</p>
    <Articles :contents="contents" />
    <p>ページ送り</p>
    <Pager :max="pagerMax" :category="categoryId"></Pager>
    <p>画像読み込み</p>
    <p><img src="~/assets/img/img_luffy.png" alt=""></p>
  </main>
</template>

<script>

export default {
  // paramsはURLにnuxt.config.jsでrouterに指定したパラメーターが入る
  // このサンプルではnuxt.config.jsのrouter :p と :categoryId の2つのパラメーターを指定している
  async asyncData({$microcms, $axios, params}) {
    const page = params.p || '1'
    const categoryId = params.categoryId
    const limit = 5
    // console.log(page)
    // console.log(categoryId)

    // カテゴリ一覽用
    // const categories = await $microcms.get({
    //   endpoint: 'categories',
    //   queries: { limit: 100 },
    // });

    const categories = await $axios.get(
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
    const { data } = await $axios.get(
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
.main {
  width: 960px;
  margin: 0 auto;
}
img {
  width: 300px;
}
</style>
