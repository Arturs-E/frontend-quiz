<template>
  <div class="form-wrapper">
    <Heading1 class="quiz-form-heading">Technical task - Quiz</Heading1>
    <form class="form" novalidate @submit.prevent="submitForm">
      <div class="form__fields-wrapper">
        <div class="form__field-wrapper">
          <TextInput
            @update:modelValue="formValues.name = $event"
            :id="'name'"
            :value="formValues.name"
            :validation-error="
              v$.$dirty && (v$.formValues.name.$error ? 'invalid' : 'valid')
            "
            :placeholder="'Please enter your name'"
            :label="'Name'"
            :focused="true"
          />
          <div class="form__input-feedback">
            <div v-if="v$.formValues.name.$error" class="form__error-message">
              {{ v$.formValues.name.$errors[0].$message }}
            </div>
          </div>
        </div>
        <FetchingError v-if="fetchingError" />
        <div v-else class="form__field-wrapper">
          <Select
            @update:modelValue="formValues.quizId = $event"
            :id="'test'"
            :value="formValues.quizId"
            :select-options="selectOptions"
            :validation-error="
              v$.$dirty && (v$.formValues.quizId.$error ? 'invalid' : 'valid')
            "
            :placeholder="'Please choose test'"
            :label="'Test'"
          />
          <div class="form__input-feedback">
            <div v-if="v$.formValues.quizId.$error" class="form__error-message">
              {{ v$.formValues.quizId.$errors[0].$message }}
            </div>
          </div>
        </div>
      </div>
      <div class="form__submit-wrapper">
        <Button variant="primary">Start</Button>
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
import Heading1 from "@/components/headings/Heading1.vue";
import TextInput from "@/components/form/text-input/TextInput.vue";
import Select from "@/components/form/select/Select.vue";

export type FormValues = {
  name: string;
  quizId: string;
};

export default defineComponent({
  name: "QuizForm",
  components: {
    Button,
    FetchingError,
    Heading1,
    TextInput,
    Select,
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
      .get("/test-quiz.php", {
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
  validations: {
    formValues: {
      name: { required },
      quizId: { required },
    },
  },
});
</script>

<style lang="scss">
@import "QuizForm";
@import "../../components/form/FormStyles";
</style>
