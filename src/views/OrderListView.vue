<template>
    <div>
        <router-link to="/"><button>Home</button></router-link>
        {{ $store.state.account.id }}
        <ul>
            <li v-for="(i, idx) in state.items" :key="idx">
                <span>{{ i.id }}</span> | 
                <span>{{ i.name }}</span>
            </li>
        </ul>
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
        });

        const load = () => {
            axios.get(`/api/cart/items/${store.state.account.id}`)
                .then(({ data })=>{
                    console.log("주문리스트", data);
                    state.items = data; 
                });
        }        

        onMounted(()=>{
            load();
        })

        return {
            state,
        }
    }
}
</script>

<style lang="css" scoped>

</style>