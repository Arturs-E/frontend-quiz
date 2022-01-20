<template>
  <div class="form__input-wrapper">
    <label :for="id" class="form__label">{{ label }}</label>
    <div class="form__select-wrapper">
      <select
        class="form__input form__select"
        :class="getValidationClass"
        :id="id"
        :value="modelValue"
        @change="$emit('update:modelValue', $event.target.value)"
      >
        <option value="" selected hidden disabled>{{ placeholder }}</option>
        <option
          v-for="option of selectOptions"
          :value="option.id"
          :key="option.id"
        >
          {{ option.title }}
        </option>
      </select>
      <span class="form__select-icon"></span>
      <font-awesome-icon
        v-if="validationError === 'valid'"
        :icon="['fas', 'check-circle']"
        class="form__input-validation-icon form__input-validation-icon--valid-select"
      />
      <font-awesome-icon
        v-if="validationError === 'invalid'"
        :icon="['fas', 'exclamation-circle']"
        class="form__input-validation-icon form__input-validation-icon--invalid-select"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import { Quiz } from "@/pages/Quiz.vue";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

export default defineComponent({
  name: "Select",
  components: {
    FontAwesomeIcon,
  },
  props: {
    modelValue: String,
    id: String,
    label: String,
    placeholder: String,
    selectOptions: Array as PropType<Quiz[]>,
    validationError: {
      type: [String, Boolean],
      validator(value: "valid" | "invalid" | false) {
        return ["valid", "invalid", false].includes(value);
      },
    },
  },
  emits: ["update:modelValue"],
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
</style>
