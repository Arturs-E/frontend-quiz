<template>
  <div class="content-container">
    <div class="content-wrapper">
      <QuizForm v-if="activeView === 'homepage'" @onSubmit="setFormValues" />
      <QuizTest
        v-else-if="activeView === 'quiz'"
        :quizId="formValues.quizId"
        @onSubmitAnswerHandler="saveQuestionAnswer"
        @onLastQuestion="goToResults"
      />
      <QuizResults
        v-else
        :formValues="formValues"
        :answers="questionAnswers"
        @onClick="retakeQuiz"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

import axios from "axios";
import QuizForm, { FormValues } from "@/views/quiz-form/QuizForm.vue";
import QuizResults from "@/views/quiz-results/QuizResults.vue";
import QuizTest from "@/views/quiz-test/QuizTest.vue";

axios.defaults.baseURL = "https://printful.com/test-quiz.php";

export type Quiz = {
  id: string;
  title: string;
};

export default defineComponent({
  name: "Quiz",
  components: {
    QuizTest,
    QuizForm,
    QuizResults,
  },
  data: () => ({
    activeView: "homepage" as "homepage" | "quiz" | "results",
    formValues: {} as FormValues,
    questionAnswers: [] as string[],
  }),
  methods: {
    setFormValues(values: FormValues) {
      this.formValues = {
        name: values.name.toLowerCase(),
        quizId: values.quizId,
      };
      this.activeView = "quiz";
    },
    saveQuestionAnswer(value: string) {
      this.questionAnswers.push(value);
    },
    goToResults() {
      this.activeView = "results";
    },
    retakeQuiz() {
      this.activeView = "homepage";
      this.questionAnswers = [];
    },
  },
});
</script>

<style lang="scss">
@import "./Quiz";
</style>
