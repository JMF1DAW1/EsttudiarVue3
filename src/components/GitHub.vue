<template>
    <div>
        <!-- TODO: Crear componente GitHub -->
        <div>
            <input type="text" placeholder="introduce usuario" v-model="user" @keydown.enter="obtenerUsuario">
        </div>

        <div v-if="userValid">
            <button type="button" @click="mostrarTodo"> Mostrar Todo porque puedo</button>

                <div v-if="mostrarALL">
                    <div v-for="(item, id) in userData" :key="id">
                        {{ id }} -- {{ item }}
                    </div>
                </div>

            <div>
                <img :src="userData.avatar_url" style="width:20%">
                <a :href="userData.html_url"> URL del usuario {{ userData.login }} </a> 
                <button type="button" @click="obtenerRepositorios"> Repositorios </button>
            </div>

            <div v-if="mostrarRepo">
               <GitHubRepos :repolist="repositorios"></GitHubRepos>
            </div>
        </div>


    </div>
</template>

<script>

import GitHubRepos from "./GitHubRepos.vue"

export default {
    name: 'GitHub',
    components: {
        GitHubRepos
    },
    data: function() {
        return {
            user: "",
            userData: {},
            userValid: false,
            repositorios: {},
            mostrarRepo: false,
            mostrarALL: false,
        }
    },
    methods: {
        obtenerUsuario: function() {
            var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let headers = new Headers();
            headers.set('Authorization', 'Basic ' + btoa(userAuth + ":" + passAuth));

            var url= "https://api.github.com/users/" + this.user;

            fetch(url, {method:'GET',
                    headers: headers,
                })
            .then(res => res.json())
            .then(response => {
                console.log(response);
                this.userData = response;
                this.userValid = true;
            });
            // => console.log(json));
        },
        obtenerRepositorios: function() {
            var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let headers = new Headers();
            headers.set('Authorization', 'Basic ' + btoa(userAuth + ":" + passAuth));

            var url= this.userData.repos_url;

            fetch(url, {method:'GET',
                    headers: headers,
                })
            .then(res => res.json())
            .then(response => {
                console.log(response);
                this.repositorios = response;
                this.mostrarRepo = true;
            });
        }, 

        mostrarTodo: function() {
            this.mostrarALL = !this.mostrarALL
        }
    }
}
</script>

<style scoped>
</style>
