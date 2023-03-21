<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <header>
      <div id="nav">
        <button><router-link to="/">홈</router-link></button>
        <button><router-link to="/login">로그인</router-link></button>
        <span v-if="$store.state.account.id"><button><router-link to="/addorder">제품주문</router-link></button></span>
      </div>
    </header>
  </div>
</template>

<script>
import store from "@/store/store"

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  setup() {

    return {
      store
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
