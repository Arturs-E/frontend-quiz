<template>
  <div class="quiz-container">
    <div class="quiz-wrapper">
      <QuizForm v-if="activeView === 'homepage'" @onSubmit="setFormValues" />
      <QuizTest
        v-else-if="activeView === 'quiz'"
        :quizId="formValues.quizId"
        @onSubmitAnswerHandler="saveQuestionAnswer"
        @onLastQuestion="goToView('results')"
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

axios.defaults.baseURL = "https://printful.com";

export type Quiz = {
  id: string;
  title: string;
};

type ActiveView = "homepage" | "quiz" | "results";

export default defineComponent({
  name: "Quiz",
  components: {
    QuizTest,
    QuizForm,
    QuizResults,
  },
  data: () => ({
    activeView: "homepage" as ActiveView,
    formValues: {} as FormValues,
    questionAnswers: [] as string[],
  }),
  methods: {
    setFormValues({ name, quizId }: FormValues) {
      this.formValues = {
        name: name.toLowerCase(),
        quizId,
      };
      this.goToView("quiz");
    },
    saveQuestionAnswer(value: string) {
      this.questionAnswers.push(value);
    },
    goToView(view: ActiveView) {
      this.activeView = view;
    },
    retakeQuiz() {
      this.goToView("homepage");
      this.questionAnswers = [];
    },
  },
});
</script>

<style scoped lang="scss">
@import "./Quiz";
</style>
