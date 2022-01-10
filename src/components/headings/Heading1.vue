<template>
  <h1 class="heading1">
    <slot></slot>
  </h1>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Heading1",
});
</script>

<style scoped lang="scss">
.heading1 {
  font-size: 44px;
  font-weight: bold;
}
</style>
