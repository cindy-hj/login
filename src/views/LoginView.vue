<template>
    <div>
        <div v-if="!$store.state.account.id">
            <h2><label for="">로그인</label></h2>
            <input type="email" @keyup.enter="submit" v-model="state.form.email" placeholder="아이디를 입력하세요."><br>
            <input type="password" @keyup.enter="submit" v-model="state.form.password" placeholder="패스워드를 입력하세요."><br>
            <div class="login">
                <button @click="submit()">Sign In</button>
                <router-link to="/"><button>home</button></router-link>
            </div>
        </div>

        <div class="logout" v-else>
            <button><a to="/" @click="logout()">logout</a></button>
            <router-link to="/"><button>home</button></router-link>  
            <router-link to="/addorder"><button>제품주문</button></router-link>
        </div>
    </div>
</template>
<script>
import { computed, reactive } from 'vue';
import axios from 'axios';
import { useStore } from 'vuex'

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
