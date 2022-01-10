<template>
  <h1 class="heading2" :class="`heading2--${variant}`">
    <slot></slot>
  </h1>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Heading2",
  props: {
    variant: {
      type: String,
      validator(value: string) {
        return ["capitalized"].includes(value);
      },
    },
  },
});
</script>

<style scoped lang="scss">
.heading2 {
  font-size: 36px;
  font-weight: bold;

  &--capitalized {
    text-transform: capitalize;
  }
}
</style>
