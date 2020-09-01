<template>
  <div class="input-group">
    <input
      id="btn-input"
      type="text"
      name="message"
      class="form-control input-sm"
      placeholder="Type your message here..."
      v-model="newMessage"
      @keyup.enter="sendMessage"
    />

    <span class="input-group-btn">
      <button class="btn btn-primary btn-sm" id="btn-chat" @click="sendMessage">Send</button>
    </span>
  </div>
</template>

<script>
export default {
  props: ["user"],

  data() {
    return {
      newMessage: "",
    };
  },

  methods: {
    created() {
      this.fetchMessages();
      Echo.private("chat").listen("MessageSent", (e) => {
        this.messages.push({
          message: e.message.message,
          user: e.user,
        });
      });
    },
    sendMessage() {
      this.$emit("messagesent", {
        user: this.user,
        message: this.newMessage,
      });

      this.newMessage = "";
    },
    addMessage(message) {
      this.messages.push(message);

      axios.post("/messages", message).then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>


