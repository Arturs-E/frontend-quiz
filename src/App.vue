<template>
  <Quiz />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Quiz from "@/pages/Quiz.vue";

export default defineComponent({
  name: "App",
  components: {
    Quiz,
  },
});
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap");

#app {
  font-family: "Inter", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
