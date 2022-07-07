<template>
  <b-row
    justify="center"
    no-gutters
  >
    <p
      v-if="isSystemMessage"
      class="font-italic system"
      style="float: left; padding: 15px; text-align: center; width: 100%;"
    >
      {{ message.text }}
    </p>
    <b-col
      v-else
      class="msg-wrapper"
    >
      <b-row
        no-gutters
        justify="space-between"
        class="msg"
        :class="{ owner }"
      >
        <b-col>
          <span v-if="!owner" class="font-weight-bold">{{ message.username }}</span>
          <p class="mb-0">
            {{ message.text }}
          </p>
        </b-col>
        <b-col cols="auto">
          <span class="msg__date ml-3">{{ message.time }}</span>
        </b-col>
      </b-row>
    </b-col>
  </b-row>
</template>

<script>
export default {
  props: {
    message: {
      type: Object,
      default: () => {},
    },
    owner: {
      type: Boolean,
    },
  },
  computed: {
    isSystemMessage() {
      return this.message.username === "admin";
    },
  },
};
</script>

<style lang="scss" scoped>
.system {
  margin-bottom: 1rem;
  color: #000;
  align-content: center;

  p {
    margin-bottom: 1rem;
  }
}

.msg-wrapper {
  display: flex;
  flex-direction: column;
}

.msg {
  padding: 1rem;
  width: 60%;
  margin: 0 1rem;
  box-shadow: 0 1px 0 0 rgba(50, 50, 50, 0.3);
  border-radius: 4px;
  background: #F6F6F6;
  color: #000;
  position: relative;
  word-break: break-all;
  margin-bottom: 1rem;

  &__date {
    text-decoration: underline;
  }
}

.owner {
  background: #5DB075;
  color: #fff;
  align-self: flex-end;
}

@media (max-width: 400px) {
  .msg {
    width: 90%;
  }
}
</style>
