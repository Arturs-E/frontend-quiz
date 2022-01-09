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
@import "App";
</style>
