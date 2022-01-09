<template>
  <div class="form-wrapper">
    <h1 class="quiz-form-heading">Technical task - Quiz</h1>
    <form class="form" novalidate @submit.prevent="submitForm">
      <div class="form__fields-wrapper">
        <div class="form__field-wrapper">
          <label for="name" class="form__label">Name</label>
          <input
            type="text"
            class="form__input"
            :class="
              v$.$dirty &&
              (v$.formValues.name.$error
                ? 'form__input--invalid'
                : 'form__input--valid')
            "
            id="name"
            placeholder="Please enter your name"
            ref="nameRef"
            v-model.trim="formValues.name"
          />
          <div class="form__input-feedback">
            <div v-if="v$.formValues.name.$error" class="form__error-message">
              {{ v$.formValues.name.$errors[0].$message }}
            </div>
          </div>
        </div>
        <FetchingError v-if="fetchingError" />
        <div v-else class="form__field-wrapper">
          <label for="test" class="form__label">Test</label>
          <div class="form__select-wrapper">
            <select
              class="form__input form__select"
              :class="
                v$.$dirty &&
                (v$.formValues.quizId.$error
                  ? 'form__input--invalid'
                  : 'form__input--valid')
              "
              id="test"
              v-model="formValues.quizId"
            >
              <option value="" selected hidden disabled>
                Please choose test
              </option>
              <option
                v-for="option of selectOptions"
                :value="option.id"
                :key="option.id"
              >
                {{ option.title }}
              </option>
            </select>
            <span class="form__select-icon"></span>
          </div>
          <div class="form__input-feedback">
            <div v-if="v$.formValues.quizId.$error" class="form__error-message">
              {{ v$.formValues.quizId.$errors[0].$message }}
            </div>
          </div>
        </div>
      </div>
      <div class="form__submit-wrapper">
        <Button additional-class="primary">Start</Button>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";
import { Quiz } from "@/pages/Quiz.vue";
import Button from "@/components/buttons/Button.vue";
import FetchingError from "@/components/fetching-error/FetchingError.vue";

export type FormValues = {
  name: string;
  quizId: string;
};

export default defineComponent({
  name: "QuizForm",
  components: {
    Button,
    FetchingError,
  },
  data: () => ({
    v$: useVuelidate(),
    fetchingError: false,
    loading: true,
    selectOptions: [] as Quiz[],
    formValues: {
      name: "",
      quizId: "",
    } as FormValues,
  }),
  emits: ["onSubmit"],
  created() {
    axios
      .get("", {
        params: {
          action: "quizzes",
        },
      })
      .then(({ data }) => {
        this.selectOptions = data;
        this.loading = false;
      })
      .catch((error) => {
        if (error) {
          this.fetchingError = true;
        }
      });
  },
  mounted() {
    const inputField = this.$refs.nameRef as HTMLInputElement;
    inputField.focus();
  },
  methods: {
    submitForm() {
      this.v$.$validate();
      if (this.v$.$error) {
        return;
      }

      this.$emit("onSubmit", this.formValues);
      this.formValues = {
        name: "",
        quizId: "",
      };
    },
  },
  validations() {
    return {
      formValues: {
        name: { required },
        quizId: { required },
      },
    };
  },
});
</script>

<style lang="scss">
@import "QuizForm";
</style>
