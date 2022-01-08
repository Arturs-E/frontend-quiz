<template>
  <div class="content-container">
    <div class="content-wrapper">
      <Homepage v-if="activeView === 'homepage'" @onSubmit="setFormValues" />
      <QuizView
        v-else-if="activeView === 'quiz'"
        :quizId="formValues.quizId"
        @onSubmitAnswerHandler="saveQuestionAnswer"
        @onLastQuestion="goToResults"
      />
      <Results
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
import Homepage, { FormValues } from "@/views/Homepage/Homepage.vue";
import QuizView from "@/views/QuizView/QuizView.vue";
import Results from "@/views/Results/Results.vue";
import axios from "axios";

axios.defaults.baseURL = "https://printful.com/test-quiz.php";

export type Quiz = {
  id: string;
  title: string;
};

export default defineComponent({
  name: "Quiz",
  components: {
    QuizView,
    Homepage,
    Results,
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
