<template>
  <div>
        <h1>Contact</h1>
    <p>例は文章ライセンスと引用さ文章ませでため、引用するられる作風を既存者自由の投稿両国をするれてはさた、記事の記事は、引用作らページを参照することに関する発揮必要でならているたです。</p>
    <form name="contact" method="POST" data-netlify="true" data-netlify-recaptcha="true" netlify-honeypot="bot-field" action="/contact/">
      <p class="hidden">
        <label>こちらの項目は入力しないでください: <input name="bot-field" /></label>
      </p>
      <p>
        <label>お名前: <input type="text" name="name" /></label>
      </p>
      <p>
        <label>メールアドレス: <input type="email" name="email" /></label>
      </p>
      <div data-netlify-recaptcha="true"></div>
      <p>
        <label>お問い合わせ内容: <textarea name="message"></textarea></label>
      </p>
      <p>
        <button type="submit">送信</button>
      </p>
    </form>
  </div>
</template>

<script>
export default {

}
</script>

<style>

</style>
