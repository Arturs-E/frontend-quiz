<template>
  <h1 class="heading1">Technical task</h1>
  <form class="form" novalidate @submit.prevent="submitForm">
    <div class="form__input-wrapper">
      <div>
        <label for="name" class="form-label form-label--bold">Name</label>
        <input
          type="text"
          class="form-control"
          :class="inputValidityClass.name"
          id="name"
          placeholder="Please enter your name"
          aria-describedby="nameValidationFeedback"
          ref="nameRef"
          v-model.trim="formValues.name"
        />
        <div class="invalid-feedback">
          Please enter your name. Name can't contain numbers.
        </div>
        <div id="nameValidationFeedback" class="valid-feedback">
          Looks good!
        </div>
      </div>
      <div>
        <label for="test" class="form-label form-label--bold">Test</label>
        <select
          class="form-select"
          :class="inputValidityClass.test"
          id="test"
          aria-label="test"
          aria-describedby="selectValidationFeedback"
          v-model="formValues.quizId"
        >
          <option value="" selected hidden disabled>Please choose test</option>
          <option
            v-for="option of selectOptions"
            :value="option.id"
            :key="option.id"
          >
            {{ option.title }}
          </option>
        </select>
        <div id="selectValidationFeedback" class="invalid-feedback">
          Please choose one of the test options.
        </div>
        <div class="valid-feedback">Looks good!</div>
      </div>
    </div>
    <div class="form__submit-wrapper">
      <button type="submit" class="btn btn-primary">Start</button>
    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import { Quiz } from "@/pages/Quiz.vue";

export type FormValues = {
  name: string;
  quizId: string;
};

export default defineComponent({
  name: "Homepage",
  data: () => ({
    loading: true,
    selectOptions: [] as Quiz[],
    formValues: {
      name: "",
      quizId: "",
    } as FormValues,
    inputValidityClass: {
      name: "",
      test: "",
    },
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
      });
  },
  mounted() {
    const inputField = this.$refs.nameRef as HTMLInputElement;
    inputField.focus();
  },
  methods: {
    submitForm() {
      let validity = true;

      if (!this.formValues.name || /\d/g.test(this.formValues.name)) {
        this.inputValidityClass.name = "is-invalid";
        validity = false;
      } else {
        this.inputValidityClass.name = "is-valid";
      }

      if (!this.formValues.quizId) {
        this.inputValidityClass.test = "is-invalid";
        validity = false;
      } else {
        this.inputValidityClass.test = "is-valid";
      }

      if (validity) {
        this.$emit("onSubmit", this.formValues);
        this.formValues = {
          name: "",
          quizId: "",
        };
      }
    },
  },
});
</script>

<style lang="scss">
@import "./Homepage";
</style>
