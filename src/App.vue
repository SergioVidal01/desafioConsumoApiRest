<template>
  <div id="app">
    <h1>Chat entre Desconocidos</h1>
    <Chat :user1="user1" :user2="user2" />
  </div>
</template>

<script>
import axios from 'axios';
import Chat from './components/Chat.vue';

export default {
  name: 'App',
  components: {
    Chat
  },
  data() {
    return {
      user1: null,
      user2: null
    };
  },
  mounted() {
    // Obtenemos dos usuarios aleatorios de la API
    Promise.all([
      axios.get('https://randomuser.me/api/'),
      axios.get('https://randomuser.me/api/')
    ])
    .then(([response1, response2]) => {
      this.user1 = response1.data.results[0];
      this.user2 = response2.data.results[0];
    })
    .catch(error => {
      console.error("Hubo un error al obtener los usuarios:", error);
    });
  }
};
</script>

<style>
/* Estilos globales */
#app {
  text-align: center;
}
</style>
