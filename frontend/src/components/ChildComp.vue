<script>
export default {
  emits: ['response'],
  created() {
    this.$emit('response', 'hello from child') // 発行されるイベントを宣言する
  },
  // this.$emit()の第一引数はイベント名
  // 追加の引数は、イベントリスナーに渡される
  props: {
    msg: String,
  }
}
</script>
<!--
子コンポーネントは、親コンポーネントからpropsを開始て入力を受け取ることができる
まず、受け入れるpropsを宣言する必要がある

一旦、宣言されると、msgプロパティはthisに出力され、子コンポーネントのテンプレートで使用することができる
-->

<template>
  <h2>Child component</h2>
</template>