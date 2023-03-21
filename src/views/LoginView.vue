<template>
    <div>
        <div v-if="!$store.state.account.id">
            <h2><label for="">Sign In</label></h2>
            <input type="email" @keyup.enter="submit" v-model="state.form.email" placeholder="id"><br>
            <input type="password" @keyup.enter="submit" v-model="state.form.password" placeholder="password"><br>
            <div class="login">
                <button @click="submit()">Sign In</button>
                <router-link to="/"><button>home</button></router-link>
            </div>
        </div>

        <div class="logout" v-else>
            <button><a to="/" @click="logout()">Sign Out</a></button>
            <router-link to="/"><button>Home</button></router-link>  
            <router-link to="/addorder"><button>Order</button></router-link>
        </div>
    </div>
</template>
<script>
import { computed, reactive } from 'vue';
import axios from 'axios';
import { useStore } from 'vuex'
import router from '@/router';

export default {
    setup() {
        const state = reactive({
            form: {
                email: null,
                password : null,
            },
            id: null,
        });

        state.id = computed(()=>store.state.account.id);
        const store = useStore();

        //로그인 제출
        const submit = () => {
            axios.post("/api/account/login",state.form).then((res) =>{
                window.alert("로그인 하였습니다.");
                console.log('결과',res);
                store.commit('setAccount', res.data);
                router.push({path:"/"});
            }).catch((error)=>{
                window.alert("로그인에 실패하였습니다.");
                console.log(error);
            });
        }

        const logout = () => {
            axios.post("/api/account/logout").then(()=>{
                window.alert("로그아웃 하였습니다.");
                store.commit('setAccount',0);
            }).catch((err)=>{
                console.log(err);
            })
        }

        return{
            state,
            submit,
            logout
        }
    },
}
</script>

<style>
    button{
        font-size: 15px;
        border:none;
        background: rgb(255, 247, 230);
        color: #ffb444;
        font-weight: bold;
        cursor: pointer;
        margin: 20px 5px;
        border-radius: 5px;
        padding: 10px;
    }
    .login {
        display: inline-block;
    }
    .logout {
        display: inline-block;
    }
</style>
