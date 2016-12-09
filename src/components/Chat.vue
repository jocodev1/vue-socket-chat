<template>
  <div class="chat">
    <p>MY USERNAME: <i>{{ username }}</i></p>
    <div class="chat-window">
      <p v-for="message in messages">
        <b>{{ message.username }}</b> <span class="gray-out"><i>('Dec 09 2016 00:00:00')</i></span>: &nbsp; {{ message.msg }}
      </p>
    </div>
    <p>
      <input type="text" v-model="newMsg"/>
    </p>
    <button type="button" v-on:click="send()">Send</button>
  </div>
</template>

<script>
export default {
  name: 'chat',
  data () {
    return {
      messages: [],
      newMsg: null,
      username: null
    }
  },
  sockets: {
    getMyUsername (username) {
      this.username = username
    },
    resetChat () {
      this.messages = []
    },
    updateMsg (data) {
      this.messages.push(data)
    }
  },
  methods: {
    /**
     * Resets new message in text input
     * @return void
     */
    resetNewMsg () {
      this.newMsg = null
    },

    /**
     * Sends new message through socket
     * @return void
     */
    send () {
      this.$socket.emit('send', {
        username: this.username,
        msg: this.newMsg
      })
      this.resetNewMsg()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.chat-window {
  text-align: left;
  padding: 15px;
  margin: 0 auto;
  width: 100%;
  max-width: 800px;
  min-height: 300px;
  overflow-y: auto;
  border: 1px solid #333333;
}
.chat-window p {
  font-size: 0.8em;
}
h1, h2 {
  font-weight: normal;
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
