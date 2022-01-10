<template>
  <h1 class="heading4" :class="`heading4--${alignment}`">
    <slot></slot>
  </h1>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Heading4",
  props: {
    alignment: {
      type: String,
      validator(value: string) {
        return ["centered"].includes(value);
      },
    },
  },
});
</script>

<style scoped lang="scss">
.heading4 {
  font-size: 18px;
  font-weight: bold;

  &--centered {
    text-align: center;
  }
}
</style>
