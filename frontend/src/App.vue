<!--宣言的レンダリング
HTMLを拡張したテンプレート構文を用いて、JavaScriptの状態に基づいてHTMLがどのように見えるかを記述できる
状態が変更されるとHTMLは自動的に更新できる

リアクティブ
変更された時に更新のトリガーとなるような状態
Vueでは、リアクティブステートはコンポーネントに保持される

dataコンポーネントオプションを使ってリアクティブステートを宣言することができるが、オブジェクトを返す関数であるべき
-->

<!--
コンポーネント
親コンポーネントは、そのテンプレートにある別のコンポーネントを子コンポーネントとしてレンダリングすることができる
子コンポーネントとして使用するには、まずそれをインポートする
-->

<script>
import ChildComp from './components/ChildComp.vue'

let id = 0

export default {
  components: {
    ChildComp
  },
  // componentsオプションを使用して、コンポーネントを登録する必要がある
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
        { id: id++, text: 'Learn HTML', done: true },
        { id: id++, text: 'Learn JavaScript', done: true },
        { id: id++, text: 'Learn Vue', done: false }
      ],
      hideCompleted: false,

      todoId: 1,
      todoData: null,

      greeting: 'Hello from parent',
      childMsg: 'No child msg yet'
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
      this.todos.push({ id: id++, text: this.newTodo, done: false })
      this.newTodo = ''
    },

    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    },

    incrementToDoId() {
      this.todoId++
    },

    async fetchData() {
      this.todoData = null
      const res = await fetch(
          `https://jsonplaceholder.typicode.com/todos/${this.todoId}`
      )
      this.todoData = await res.json()
    }
  },
  // 算出プロパティ
  // 他のプロパティからリアクティブに算出されたプロパティをcomputedオプションを使用して宣言
  computed: {
    filteredTodos() {
      return this.hideCompleted
        ? this.todos.filter((t) => !t.done)
        : this.todos
    },
  },

  mounted() {
    // マウントした後のコードを実施したい場合、mountedのoptionが使える
    // コンポーネントがマウントされました

    // ライフサイクルフックと呼ばれ、コンポーネントライフサイクルの特定の時に呼び出されるコールバックを登録できる
    // mountedは、VueインスタンスがDOMにマウントされた直後に呼び出されるフック。
    // つまり、VueインスタンスがDOMに挿入された直後に、このフックが呼び出される

    this.$refs.p.textContent = 'mounted!'
    // refでmountを呼び出し、pという要素のtextContent属性を指定
    // DOMを直接操作
    this.fetchData()
  },
//  メソッドの中では、thisを使ってコンポーネントインスタンスにアクセスできる
//  コンポーネントインスタンスはdataによって宣言されたデータプロパティを公開
//  これらのプロパティを変更することで、コンポーネントの状態を更新できる

  watch: {
    count(newCount) {
      // watchオプションでcountプロパティを監視している
      // watchコールバックは、countが変更された時に呼び出され、新しい値を引数として受け取る
      console.log(`new count is: ${newCount}`)
     },

    // todoIdプロパティを監視している
    todoId() {
      this.fetchData()
    }
  },
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
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
<!--      todo.doneがdoneがtrueの場合、クラス属性にdoneが入る仕組み-->
      <button @click="removeTodo(todo)">削除</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show All' : 'Hide Completed' }}
  </button>
<!--
各ToDoオブジェクトに一意のidを与え、それを各<li>の特別なkey属性としてバインドしている
このkeyにより、Vueは各<li>を配列内の対応するオブジェクトの位置に合わせて正確に移動できる

リストを更新するには、2つの方法がある
1. 配列の変更メソッドを呼び出す
  this.todos.push(newTodo)

2. 配列を新しいものに置き換える
  this.todos = this.todos.filter(/* ... */)

-->

<!--
算出プロパティ
各ToDoにトグル機能を追加する
各ToDoオブジェクトにdoneプロパティを追加して、チェックボックスにそれをバインドするためにv-modelを使う
-->

<!--
テンプレート参照
特別なref属性（例：テンプレート内の要素）を使って、テンプレートの参照が要求できる
要素がthis.$refsはthis.$refs.pとして公開されるが、コンポーネントがマウントされた後でないとアクセスできない

-->
  <p ref="p">hello</p>

  <p>Todo id: {{ todoId }}</p>
  <button @click="incrementToDoId">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>

<!--
ChildCompコンポーネントをテンプレート内で使用する

親はv-onを使って子が発行するイベントを購読できる
ここでは、ハンドラーは子のemit呼び出しから追加の引数を受け取り、それをローカルステートに割り当てている
-->
  <ChildComp />
  <ChildComp :msg="greeting" />
  <ChildComp @response="(msg) => childMsg = msg" />
  <p>{{ childMsg }}</p>
<!--
親は属性と同じように、propを子に渡すことができる
動的な値を渡すには、v-bindという構文も使える
-->
</template>

<style>
#aaa {
  color: #42b983;
}
.title {
  color: brown;
}
.done {
  text-decoration: line-through;
}
</style>