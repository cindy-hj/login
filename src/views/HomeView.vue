<template>
<div v-if=state.items>
  <div v-if="!store.state.account.id">
    <router-link to="/login"><button>Sign In</button></router-link>
    <div v-for="(item, idx) in state.items" :key="idx">
      <CardInfo :item="item"/>
    </div>
  </div>
  <div v-else>
    <button @click="logout()">logout</button>
    <router-link to="/addorder"><button>Order</button></router-link>
    <div v-for="(item, idx) in state.items" :key="idx">
      <CardInfo :item="item"/>
    </div>
  </div>
</div>
</template>

<script>
import CardInfo from '../components/CardInfo.vue'
import { reactive } from 'vue'
import axios from 'axios'
import lib from "@/lib/lib"
import store from "@/store/store"

export default {
  name: 'HomeView',
  components: {
    CardInfo
  },

  setup(){
    const state = reactive({
      items: []
    });

    const logout = () => {
      axios.post("/api/account/logout").then(()=>{
        window.alert("로그아웃 하였습니다.");

        store.commit('setAccount',0);
      }).catch((err)=>{
        console.log(err);
      })
    };

    axios.get("/api/items").then(({data})=>{
      state.items = data;
      console.log("메인페이지",data);
    });

    return {
      state,
      lib,
      store,
      logout
    }
  }
}
</script>

<style lang="css" scoped>
  button{
    font-size: 15px;
    border:none;
    background: rgb(255, 247, 230);
    color: #ffb444;
    font-weight: bold;
    cursor: pointer;
    margin-top: 20px;
    border-radius: 5px;
    padding: 10px;
  }
</style>
