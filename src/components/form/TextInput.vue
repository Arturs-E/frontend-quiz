<template>
  <div class="form__input-wrapper">
    <label :for="id" class="form__label">{{ label }}</label>
    <div class="form__text-input-wrapper">
      <input
        type="text"
        class="form__input"
        :class="getValidationClass"
        :placeholder="placeholder"
        :id="id"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        ref="reference"
      />
      <font-awesome-icon
        v-if="validationError === 'valid'"
        :icon="['fas', 'check-circle']"
        class="form__input-validation-icon form__input-validation-icon--valid"
      />
      <font-awesome-icon
        v-if="validationError === 'invalid'"
        :icon="['fas', 'exclamation-circle']"
        class="form__input-validation-icon form__input-validation-icon--invalid"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

export default defineComponent({
  name: "TextInput",
  components: {
    FontAwesomeIcon,
  },
  props: {
    modelValue: String,
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
@import "FormStyles";
.form__input-wrapper {
  display: grid;
  grid-template-columns: 1fr;
  row-gap: 4px;
}

.form__text-input-wrapper {
  position: relative;
  width: 100%;
}
</style>
