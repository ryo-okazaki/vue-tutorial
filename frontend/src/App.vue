<!--宣言的レンダリング
HTMLを拡張したテンプレート構文を用いて、JavaScriptの状態に基づいてHTMLがどのように見えるかを記述できる
状態が変更されるとHTMLは自動的に更新できる

リアクティブ
変更された時に更新のトリガーとなるような状態
Vueでは、リアクティブステートはコンポーネントに保持される

dataコンポーネントオプションを使ってリアクティブステートを宣言することができるが、オブジェクトを返す関数であるべき
-->

<script>
let id = 0

export default {
  data() {
    return {
      message: "Hello World",
      dynamicId: 'aaa',
      titleClass: 'title',
      count: 0,
      text: '',
      awesome: false,

      newTodo: '',
      todos: [
        { id: id++, text: 'Learn HTML' },
        { id: id++, text: 'Learn JavaScript'},
        { id: id++, text: 'Learn Vue'}
      ]
    }
  },
  methods: {
    increment() {
    //  コンポーネントの状態を更新する
      this.count++
    },

    onInput(e) {
      console.log(e)
    // v-on ハンドラーはネイティブDOMのイベントを引数として受け取る
      this.text = e.target.value
    },

    toggle() {
      this.awesome = !this.awesome
    },

    addTodo() {
      this.todos.push({ id: id++, text: this.newTodo})
      this.newTodo = ''
    },

    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    }
  }
//  メソッドの中では、thisを使ってコンポーネントインスタンスにアクセスできる
//  コンポーネントインスタンスはdataによって宣言されたデータプロパティを公開
//  これらのプロパティを変更することで、コンポーネントの状態を更新できる
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

<!--
イベントリスナー
v-onディレクティブを使うことで、DOMイベントを購読することができる
-->
  <button v-on:click="increment">count is {{ count }}</button>
  <button @click="increment">count is {{ count }}</button>
<!--
incrementはmethodsオプションを使って宣言された関数を参照
-->

<!--
フォームバインディング
v-bindとv-onを一緒に使うことで、input要素に双方向バインディングを作成することができる

v-modelは<input>の値をバインドされた状態と自動的に同期するので、そのためのイベントハンドラーを使う必要はない
-->
  <input :value="text" @input="onInput">
  <input v-model="text">
  <p>{{ text }}</p>

<!--
条件付きレンダリング
要素を条件付きでレンダリングする際に、v-ifディレクティブを使用できる
-->
  <button @click="toggle">toggle</button>
  <h1 v-if="awesome">Vue is awesome</h1>
  <h1 v-else>Oh no😢</h1>

<!--
リストレンダリング
v-forディレクティブを使用すると、配列を基にした要素のリストをレンダリングできる
-->
  <input v-model="newTodo">
  <button @click="addTodo">add Todo</button>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">削除</button>
    </li>
  </ul>
<!--
各ToDoオブジェクトに一意のidを与え、それを各<li>の特別なkey属性としてバインドしている
このkeyにより、Vueは各<li>を配列内の対応するオブジェクトの位置に合わせて正確に移動できる

リストを更新するには、2つの方法がある
1. 配列の変更メソッドを呼び出す
  this.todos.push(newTodo)

2. 配列を新しいものに置き換える
  this.todos = this.todos.filter(/* ... */)

-->
</template>

<style>
#aaa {
  color: #42b983;
}
.title {
  color: brown;
}
</style>