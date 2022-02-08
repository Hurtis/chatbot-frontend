<template>
  <form @submit.prevent="formSubmited()">
    <input
      type="text"
      placeholder="Napíš správu ..."
      class="textarea"
      v-model="myText"
    />
  </form>
</template>

<script>
export default {
  name: "ChatForm",
  data() {
    return {
      myText: "",
    };
  },
  methods: {
    formSubmited() {
      if (this.myText != "") {
        this.$emit("formSent", this.myText);
        this.myText = "";
      }
    },
  },
};
</script>