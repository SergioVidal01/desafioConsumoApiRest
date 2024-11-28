<template>
  <div class="chat-container" v-if="user1 && user2">
    <!-- Contenedor del chat -->
    <div class="chat-box-container">
      
      <!-- Contenedor de usuario 1 -->
      <div class="user-container-left">
        <div class="user-image">
          <img :src="user1.picture.large" alt="User 1" />
        </div>
        <div class="user-name">{{ user1.name.first }}</div>
        <input v-model="messageUser1" type="text" placeholder="Escribe un mensaje" class="user-input" />
        
        <!-- Selector de color para fondo del mensaje de user1 -->
        <input v-model="messageUser1Color" type="color" class="color-picker" />
        
        <button @click="sendMessage(1)">Enviar</button>
      </div>

      <!-- Caja de chat central -->
      <div class="chat-box">
        <!-- Mostrar mensajes con v-for -->
        <div v-for="(message, index) in messages" :key="index" class="message" :style="messageStyle(message)">
          <img :src="message.userImage" alt="user" class="user-image" />
          <strong>{{ message.user }}:</strong> {{ message.text }}
        </div>
      </div>

      <!-- Contenedor de usuario 2 -->
      <div class="user-container-right">
        <div class="user-image">
          <img :src="user2.picture.large" alt="User 2" />
        </div>
        <div class="user-name">{{ user2.name.first }}</div>
        <input v-model="messageUser2" type="text" placeholder="Escribe un mensaje" class="user-input" />
        
        <!-- Selector de color para fondo del mensaje de user2 -->
        <input v-model="messageUser2Color" type="color" class="color-picker" />
        
        <button @click="sendMessage(2)">Enviar</button>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'Chat',
  props: {
    user1: Object,
    user2: Object
  },
  data() {
    return {
      messages: [],
      messageUser1: '',
      messageUser2: '',
      messageUser1Color: '#f0f0f0', // Color por defecto para el fondo de mensaje de user1
      messageUser2Color: '#f0f0f0', // Color por defecto para el fondo de mensaje de user2
      currentUser: 1 // 1 para user1, 2 para user2
    };
  },
  methods: {
    sendMessage(userNumber) {
      let messageText = '';
      let user = null;
      let userColor = '';

      // Determinar qué usuario está enviando el mensaje
      if (userNumber === 1) {
        messageText = this.messageUser1;
        user = this.user1;
        userColor = this.messageUser1Color; // Obtener el color del input de user1
        this.messageUser1 = ''; // Limpiar la casilla de mensaje de user1
        this.messageUser1Color = '#f0f0f0'; // Resetear color al valor por defecto
      } else {
        messageText = this.messageUser2;
        user = this.user2;
        userColor = this.messageUser2Color; // Obtener el color del input de user2
        this.messageUser2 = ''; // Limpiar la casilla de mensaje de user2
        this.messageUser2Color = '#f0f0f0'; // Resetear color al valor por defecto
      }

      if (messageText.trim() !== '') {
        this.messages.push({
          text: messageText,
          user: `${user.name.first} ${user.name.last}`,
          userImage: user.picture.thumbnail, // Imagen del usuario
          color: userColor // Aplicar el color seleccionado
        });
        
        // Alternar entre los usuarios para el siguiente mensaje
        this.currentUser = this.currentUser === 1 ? 2 : 1;
      }
    },
    messageStyle(message) {
      return {
        backgroundColor: message.color,
        color: message.color === '#000000' ? '#fff' : '#000',
        borderRadius: '15px', // Borde redondeado para simular burbuja de chat
        padding: '8px 12px', // Reducir el tamaño del padding
        marginBottom: '8px', // Reducir el espacio entre los mensajes
        maxWidth: '75%', // Limitar el ancho máximo de los mensajes
        alignSelf: message.user === `${this.user1.name.first} ${this.user1.name.last}` ? 'flex-start' : 'flex-end', // Alinear mensajes a izquierda o derecha
        border: '1px solid #ccc', // Borde sutil
        overflowWrap: 'break-word' // Asegurar que el texto largo no se salga
      };
    }
  }
};
</script>

<style scoped>
.chat-container {
  width: 90%;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
}

.chat-box-container {
  display: flex;
  align-items: flex-start;
  justify-content: center;
  width: 100%;
  position: relative;
}

.chat-box {
  width: 60%; /* Ancho más grande para el chat */
  height: 400px; /* Aumento la altura del chat */
  overflow-y: scroll;
  border: 1px solid #ccc;
  padding: 10px;
  background-color: #f9f9f9;
  position: relative;
  margin-left: 10px;
  margin-right: 0px;
  display: flex;
  flex-direction: column;
}

.user-container-left{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.user-image {
  width: 60px; /* Reducir el tamaño de la imagen */
  height: 60px;
  border-radius: 50%;
  margin-bottom: 10px;
}

.user-container-left .user-image {
  position: relative;
  left: 0px; /* Imagen de usuario 1 pegada al lado izquierdo */
}

.user-container-right .user-image {
  position: relative;
  right: 0px; /* Imagen de usuario 2 pegada al lado derecho */
}

.user-name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
  margin-top: 70px;
}

.user-input {
  width: 80%;
  padding: 5px;
  margin-bottom: 10px;
}

.color-picker {
  width: 80%;
  margin-bottom: 10px;
}

button {
  padding: 5px 10px;
}

.message {
  display: flex;
  align-items: center;
  padding: 5px;
  margin-bottom: 8px; /* Reducir el espacio entre los mensajes */
  word-wrap: break-word; /* Asegurar que el texto largo no se salga */
}

.message img {
  border-radius: 50%;
  width: 25px; /* Reducir el tamaño de la imagen del usuario en el mensaje */
  height: 25px;
  margin-right: 8px;
}

input[type="text"] {
  width: 80%;
  padding: 5px;
}

h3 {
  margin-bottom: 5px;
  font-size: 14px;
}
</style>
