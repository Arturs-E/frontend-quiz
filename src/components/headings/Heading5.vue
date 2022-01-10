<template>
  <h1 class="heading5">
    <slot></slot>
  </h1>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Heading5",
});
</script>

<style scoped lang="scss">
.heading5 {
  font-size: 14px;
  font-weight: bold;
}
</style>
