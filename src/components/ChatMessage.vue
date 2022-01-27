<template>
  <div class="message-wrap" v-if="myContent != ''">
    <div class="avatar"></div>
    <div class="msg-content">
      <div
        class="msg basic"
        v-for="(text, index) in myContent.split('::')"
        :key="index"
      >
        {{ text }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ChatMessage",
  props: ["myContent"],
};
</script>