<template>
  <b-container>

    <b-jumbotron header="Join Chatroom" class="text-center">
    </b-jumbotron>

    <b-form-group
      horizontal
      :label-cols="2"
      label="Username">
      <b-form-input v-model="username" placeholder="Enter your username"></b-form-input>
    </b-form-group>

    <b-form-group
      horizontal
      :label-cols="2"
      label="Room">
      <b-form-input v-model="room" placeholder="Enter your room code"></b-form-input>
    </b-form-group>

    <b-form-group>
      <b-button block style="background-color: #5DB075; outline-color: #5DB075;" @click="join()">Join</b-button>
    </b-form-group>
  </b-container>
</template>
<script>
export default {
  data() {
    return {
      username: '',
      room: ''
    }
  },
  mounted() {
    this.socket = this.$nuxtSocket({
      name: 'main',
      upgrade: false,
    })
  },
  methods: {
    join() {
      this.socket.emit('login', { username: this.username, room: this.room }, (res) => {
        console.log(res);
        const { success, message, data } = res
        if (success) {
          this.$store.commit('setUser', data);
          this.$router.push(`/room`);
        } else {
          this.$toast.info(message);
        }
      });
    }
  }
}
</script>