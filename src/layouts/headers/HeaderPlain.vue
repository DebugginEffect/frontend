<template>
    <nav class="bg-blue-600 py-2 md:py-3">
        <div class="container px-4 mx-auto md:flex md:items-center">
            <div class="flex justify-between items-center">
                <router-link class="font-bold text-xl text-white hover:text-gray-400" to="/"> Freeport </router-link>

                <button
                    id="navbar-toggle"
                    class="border border-solid border-gray-600 px-3 py-1 rounded text-white opacity-50 hover:opacity-75 md:hidden"
                >
                    <i class="fas fa-bars"></i>
                </button>
            </div>
            <div v-if="user">
                <div id="navbar-collapse" class="hidden md:flex flex-col md:flex-row md:ml-auto mt-3 md:mt-0 gap-5">
                    <router-link
                        :to="{ name: 'login' }"
                        class="bg-zinc-600 hover:bg-zinc-400 text-white font-bold py-1 px-3 rounded focus:outline-none focus:shadow-outline"
                    >Login
                    </router-link>
                    <router-link
                        :to="{ name: 'register' }"
                        class="bg-zinc-600 hover:bg-zinc-400 text-white font-bold py-1 px-3 rounded focus:outline-none focus:shadow-outline"
                    >Signup</router-link
                    >
                </div>
            </div>
        </div>
    </nav>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import axios from 'axios'
import authHeader from '../../services/auth.header'

export default defineComponent({
    name: 'HeaderPlain',
    data() {
        return {
            user: null,
            loaded: false,
        }
    },
    methods: {
        userstatus() {
            axios({
                method: 'get',
                url: '/auth/whoami',
                withCredentials: true,
                headers: authHeader(),
            })
                .then((response) => {
                 if (response.data.login == true) {
                        this.user = response.data.user
                        this.loaded = true
                    }
                })
                .catch(() => {
                    this.user = null
                })
        },
        logout() {
            localStorage.removeItem('user_token')
            localStorage.removeItem('auth_token')
            localStorage.clear()
        },
    },
})
</script>
