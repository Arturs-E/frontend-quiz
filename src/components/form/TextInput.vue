<template>
  <div class="form__input-wrapper">
    <label :for="id" class="form__label">{{ label }}</label>
    <input
      type="text"
      class="form__input"
      :class="getValidationClass"
      :placeholder="placeholder"
      :id="id"
      :value="value"
      @input="$emit('update:modelValue', $event.target.value)"
      ref="reference"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "TextInput",
  props: {
    value: String,
    id: String,
    label: String,
    placeholder: String,
    focused: Boolean,
    validationError: {
      type: [String, Boolean],
      validator(value: "valid" | "invalid" | false) {
        return ["valid", "invalid", false].includes(value);
      },
    },
  },
  emits: ["update:modelValue"],
  mounted() {
    if (this.focused) {
      const inputField = this.$refs.reference as HTMLInputElement;
      inputField.focus();
    }
  },
  computed: {
    getValidationClass() {
      if (this.validationError === false) {
        return "";
      }
      return this.validationError === "invalid"
        ? "form__input--invalid"
        : "form__input--valid";
    },
  },
});
</script>

<style scoped lang="scss">
@import "../FormStyles";
.form__input-wrapper {
  display: grid;
  grid-template-columns: 1fr;
  row-gap: 4px;
}
</style>
