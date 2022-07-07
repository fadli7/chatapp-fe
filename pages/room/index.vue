<template>
  <!-- <b-container>
    <br><br><br>
      <b-card
        :header="id"
        header-tag="header"
      >
        <div :key="`message-${index}`" v-for="(message, index) in messages">
          <p>{{ message.text }}</p>
        </div>
      </b-card>

      <b-card-footer>
        <b-row>
          <b-col col lg="10">
            <b-form-input v-model="message" placeholder="Enter your message"></b-form-input>
          </b-col>
          <b-col col lg="2">
            <b-button block variant="info" @click="send()">Send</b-button>
          </b-col>
        </b-row>
      </b-card-footer>
  </b-container> -->

  <!-- <div class="chat-wrapper">
    <div
      ref="chat"
      class="chat"
    >
      <p style="color: #000">Test</p>
      <Message
        v-for="(message, index) in messages"
        :key="`message-${index}`"
        :message="message"
        :owner="message.id === user.id"
      />
    </div>
    <div class="chat__form">
      <ChatForm />
      Tost
    </div>
  </div> -->

  <div>
    <b-container
      fluid
      style="height: 100%;"
      >
      <div class="chat-wrapper">
        <b-navbar fixed="top" variant="light" type="light">
          <!-- <b-navbar-brand class="text-center">BootstrapVue</b-navbar-brand> -->
          <b-button size="sm" variant="outline-success" @click="exit()">Exit</b-button>
          <div style="float: left; padding: 15px; text-align: center; width: 100%;">
            <h4>{{ user.room }}</h4>
          </div>
        </b-navbar>
        <div ref="chat" class="chat">
          <Message
            v-for="(message, index) in messages"
            :key="`message-${index}`"
            :message="message"
            :owner="message.id === user.id"
          />
        </div>
        <div class="chat__typing">
          
        </div>
        <div class="chat__form">
          <!-- <ChatForm /> -->
          <b-row>
            <b-col col lg="10" md="10" sm="10" xl="10" xs="10">
              <b-form-input v-model="message" placeholder="Enter your message"></b-form-input>
            </b-col>
            <b-col col lg="2" md="2" sm="2" xl="2" xs="2">
              <b-button block style="background-color: #5DB075; outline-color: #5DB075;" @click="send()">Send</b-button>
            </b-col>
          </b-row>
        </div>
      </div>
    </b-container>
  </div>
</template>
<script>
import Message from "@/components/Message";
import ChatForm from "@/components/ChatForm";
import { mapActions, mapState } from "vuex";
export default {
  components: {
    Message,
    ChatForm
  },
  data() {
    return {
      message: '',
      listChat: []
    }
  },
  computed: {
    ...mapState(["user"]),
    id() {
      return this.$route.params.id
    },
    chats: {
      get() {
        return this.listChat
      },
      set(chatObject) {
        this.listChat.push(chatObject)
      }
    },
    messages() {
      return this.$store.state.messages
    }
  },
  watch: {
    messages() {
      setTimeout(() => {
        if (this.$refs.chat) {
          this.$refs.chat.scrollTop = this.$refs.chat.scrollHeight;
        }
      }, 0);
    },
  },
  created() {
    if (!this.user.id) this.$router.push('/')
    this.socket = this.$nuxtSocket({
      name: 'main',
      upgrade: false,
    })
    this.joinRoom(this.user);
  },
  // mounted() {
    
  // },
  methods: {
    ...mapActions(["createMessage"]),
    send() {
      this.socket.emit("createMessage", {
        id: this.user.id,
        msg: this.message,
      })
      this.message = ''
    },
    joinRoom({ username, room }) {
      this.socket.emit("joinRoom", { username, room })
    },
    exit() {
      this.socket.emit("leftChat", { id: this.user.id })
      this.$router.push('/')
    }
  }
}
</script>
<style>
.chat-wrapper {
  height: 100%;
  position: relative;
  overflow: hidden;
}

.chat__form {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1rem;
  height: 80px;
}

.chat {
  position: fixed;
  top: 60px;
  right: 0;
  left: 0;
  bottom: 80px;
  padding: 1rem;
  overflow-y: auto;
  /* color: #000; */
}

.chat__typing {
  position: fixed;
  display: flex;
  bottom: 50px;
}

.chat__typing-user:not(first-child) {
  margin-left: 15px;
}
</style>