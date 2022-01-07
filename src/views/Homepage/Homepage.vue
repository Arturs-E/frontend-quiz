<template>
  <h1 class="heading1">Technical task</h1>
  <form class="form" novalidate @submit.prevent="submitForm">
    <div class="form__input-wrapper">
      <div>
        <label for="name" class="form-label">Name</label>
        <input
          type="text"
          class="form-control"
          :class="inputValidityClass.name"
          id="name"
          placeholder="Please enter your name"
          aria-describedby="nameValidationFeedback"
          ref="nameRef"
          v-model="inputValues.name"
        />
        <div class="invalid-feedback">
          Please enter your name. Name can't contain numbers.
        </div>
        <div id="nameValidationFeedback" class="valid-feedback">
          Looks good!
        </div>
      </div>
      <div>
        <select
          class="form-select"
          :class="inputValidityClass.test"
          aria-label="Select"
          aria-describedby="selectValidationFeedback"
          v-model="inputValues.id"
        >
          <option value="" selected hidden disabled>Please choose test</option>
          <option
            v-for="option of quizOptions"
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

export default defineComponent({
  name: "Homepage",
  data: () => ({
    loading: true,
    quizOptions: [] as Quiz[],
    inputValues: {
      name: "",
      id: "",
    },
    inputValidityClass: {
      name: "",
      test: "",
    },
  }),
  emits: ["onSubmit"],
  created() {
    axios
      .get("https://printful.com/test-quiz.php?action=quizzes")
      .then(({ data }) => {
        this.quizOptions = data;
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

      if (!this.inputValues.name || /\d/g.test(this.inputValues.name)) {
        this.inputValidityClass.name = "is-invalid";
        validity = false;
      } else {
        this.inputValidityClass.name = "is-valid";
      }

      if (!this.inputValues.id) {
        this.inputValidityClass.test = "is-invalid";
        validity = false;
      } else {
        this.inputValidityClass.test = "is-valid";
      }

      if (validity) {
        this.$emit("onSubmit", this.inputValues);
        this.inputValues = {
          name: "",
          id: "",
        };
      }
    },
  },
});
</script>

<style lang="scss">
@import "./Homepage";
</style>
