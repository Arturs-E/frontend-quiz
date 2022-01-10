<template>
  <h1 class="heading3">
    <slot></slot>
  </h1>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Heading3",
});
</script>

<style scoped lang="scss">
.heading3 {
  font-size: 24px;
  font-weight: bold;
}
</style>
