<script>
export default {
  props: {
    msg: String
  }
}
</script>
<!--
子コンポーネントは、親コンポーネントからpropsを開始て入力を受け取ることができる
まず、受け入れるpropsを宣言する必要がある

一旦、宣言されると、msgプロパティはthisに出力され、子コンポーネントのテンプレートで使用することができる
-->

<template>
  <h2>{{ msg || 'No props passed yet' }}</h2>
</template>