<template>
  <div class="content-container">
    <div class="content-wrapper">
      <Homepage />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Homepage from "@/views/Homepage/Homepage.vue";

export default defineComponent({
  name: "App",
  components: {
    Homepage,
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

.content-container {
  max-width: 1000px;
  margin: 0 auto;
}

.content-wrapper {
  padding: 36px 24px;
}
</style>
