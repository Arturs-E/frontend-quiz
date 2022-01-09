<template>
  <label :for="id">
    <input
      type="radio"
      name="answers"
      class="quiz__answer-input"
      ref="inputRef"
      :id="id"
      :value="id"
      @change="$emit('update:modelValue', $event.target.value)"
    />
    <span class="quiz__answer-label">{{ title }}</span>
  </label>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "QuizAnswer",
  props: ["id", "title", "modelValue"],
  emits: ["update:modelValue"],
});
</script>

<style lang="scss">
@import "./QuizAnswer";
</style>
