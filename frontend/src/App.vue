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
      message: "Hello World",
      dynamicId: 'aaa',
      titleClass: 'title'
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

  <!--
属性バインディング
Vueでは、mustaches(二重中括弧)はテキスト補間のみ使用する
動的な属性にバインドするのは、v-bindディレクティブを使う

ディレクティブはv-から始まる特別な属性。Vueテンプレートの一部
テキスト補間と同様に、ディレクティブの値はコンポーネント状態にアクセスできるJavaScript式です。
-->
  <div v-bind:id="dynamicId">export defaultのdata()でreturnされるdynamicIdの「値」を属性と対応させている</div>
<!--
コロンの後の部分（:id）はディレクティブの「引数」
ここでは要素のidはコンポーネントの状態から、dynamicId属性と同期される

v-bindは非常に頻繁に使うため、専用の省略記法がある
-->
  <div :id="titleClass">yyy</div>
</template>

<style>
#aaa {
  color: #42b983;
}
.title {
  color: brown;
}
</style>