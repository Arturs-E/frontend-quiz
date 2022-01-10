<template>
  <div class="progress-bar-heading-wrapper">
    <Heading5>Progress:</Heading5>
  </div>
  <div class="progress-bar-outer">
    <div class="progress-bar-inner" :style="{ width: `${width}%` }">
      {{ content }}
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Heading5 from "@/components/headings/Heading5.vue";

export default defineComponent({
  name: "ProgressBar",
  components: {
    Heading5,
  },
  props: ["width", "content"],
});
</script>

<style lang="scss">
@import "ProgressBar";
</style>
