<template>
  <h5 style="margin-bottom: 4px">Progress:</h5>
  <div class="progress-bar-outer">
    <div class="progress-bar-inner" :style="{ width: `${width}%` }">
      {{ content }}
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "ProgressBar",
  props: ["width", "content"],
});
</script>

<style lang="scss">
@import "ProgressBar";
</style>
