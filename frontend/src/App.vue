<!--宣言的レンダリング
HTMLを拡張したテンプレート構文を用いて、JavaScriptの状態に基づいてHTMLがどのように見えるかを記述できる
状態が変更されるとHTMLは自動的に更新できる

リアクティブ
変更された時に更新のトリガーとなるような状態
Vueでは、リアクティブステートはコンポーネントに保持される

dataコンポーネントオプションを使ってリアクティブステートを宣言することができるが、オブジェクトを返す関数であるべき
-->

<script>
export default {
  data() {
    return {
      message: "Hello World"
    }
  }
}
</script>

<!--
messageプロパティはテンプレート(<template>)内で使用可能
mustaches構文を使い、messageの値に基づいた動的なテキストをレンダリングすることができる

-->
<template>
  <h1>{{ message }}</h1>
<!--
mustacheの内側の内容は識別子やパスに限られない
有効なJavaScriptの式であれば何でも使える
-->
  <h1>{{ message.split('').reverse().join('') }}</h1>

  <h1>{{ message + ' ' + message }}</h1>
</template>