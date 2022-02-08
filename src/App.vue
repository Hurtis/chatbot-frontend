<template>
  <div>
    <div class="panel">
      <ul class="chat">
        <li
          v-for="(message, index) in messages"
          :class="message.direction"
          :key="index"
        >
          <ChatMessage :myContent="message.text" />
        </li>
      </ul>
      <ul class="chat" v-if="typing">
        <li>
          <img
            src="./assets/loading.gif"
            alt="typing"
            width="40"
            class="typing"
          />
        </li>
      </ul>
      <div class="sugest-wrapper" v-if="messages != []">
        <div
          class="btn"
          v-for="(btn, index) in messages[messages.length - 1].suggestion"
          @click="createUserMessage(btn)"
          :key="index"
        >
          {{ btn }}
        </div>
      </div>
    </div>
    <chatForm @formSent="createUserMessage($event)" />
  </div>
</template>

<script>
import axios from "axios";
import ChatMessage from "./components/ChatMessage.vue";
import ChatForm from "./components/ChatForm.vue";
export default {
  name: "App",
  components: {
    ChatMessage,
    ChatForm,
  },
  data() {
    return {
      messages: [{ text: "", suggestion: [] }],
      typing: false,
    };
  },
  methods: {
    createUserMessage(userMessage) {
      this.messages.push({
        text: userMessage,
        direction: "user",
      });
      this.fetchData(userMessage);
    },
    createBotMessage(botMessage) {
      this.messages.push({
        text: botMessage.text,
        direction: "bot",
        conversation: false,
        suggestion: botMessage.buttons,
        form: false,
      });
    },
    fetchData(myData) {
      this.typing = true;
      let unifiedData = myData.toLowerCase().trim();
      unifiedData = unifiedData
        .normalize("NFD")
        .replace(/[\u0300-\u036f]/g, "");
      axios
        .post("https://robot.hurtis.sk", {
          question: unifiedData,
          conversation: false,
        })
        .then((response) => {
          this.createBotMessage(response.data);
          this.typing = false;
        })
        .catch((error) => {
          let errorMsg = {
            text: "Niekde nastal problem. Skús to neskôr.",
            direction: "bot",
            conversation: false,
            buttons: [],
            form: false,
          };
          this.createBotMessage(errorMsg);
        });
    },
  },
  mounted() {
    this.fetchData("start");
  },
};
</script>
