<template>
  <button
    @click="$emit('button:on-click')"
    class="button"
    :class="`button--${variant}`"
    :disabled="disabled"
  >
    <slot></slot>
  </button>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Button",
  props: {
    variant: {
      type: String,
      validator(value: string) {
        return ["primary", "secondary", "disabled"].includes(value);
      },
    },
    disabled: Boolean,
  },
});
</script>

<style lang="scss">
@import "Button";
</style>
