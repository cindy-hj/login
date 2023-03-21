<template>
    <div>
        <router-link to="/orderlist"><button>Order List</button></router-link>
        {{ $store.state.account.id }}
        <div v-if="$store.state.account.id">
            <div v-for="(item, idx) in state.items" :key="idx">
                <span>{{ item.name }}</span>|
                <span>{{ item.price }}</span>|
                <span>{{ item.discountPer }}</span>
                <button @click="addToCart(item.id, $store.state.account.id)">Buy</button>
            </div>
        </div>
    </div>
</template>

<script>
import { onMounted, reactive } from 'vue'
import axios from 'axios';
import store from '@/store/store';

export default {
    setup () {
        const state = reactive({
            items : [],
            form : {
                itemId: "",
                memberId: "",
            }
        });

        const load = () => {
            axios.get("/api/items")
                .then(({data})=>{
                    console.log("주문페이지",data);
                    state.items = data;
                })
        };

        const logout = () => {
            axios.post("/api/account/logout").then(()=>{
                window.alert("로그아웃 하였습니다.");

                store.commit('setAccount',0);
            }).catch((err)=>{
                console.log(err);
            })
        }

        const addToCart = (itemId, memberId) => {
            state.form.itemId = itemId;
            state.form.memberId = memberId;
            axios.post(`/api/cart/items`, state.form)
                .then(()=>{
                    console.log("success");
                });
        };

        onMounted(()=>{
            load();
        })

        return {
            state,
            logout,
            addToCart,
        }
    }
}
</script>

<style lang="scss" scoped>

</style>