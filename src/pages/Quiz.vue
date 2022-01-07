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
        @onClick="retakeQuiuz"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Homepage, { FormValues } from "@/views/Homepage/Homepage.vue";
import QuizView from "@/views/QuizView/QuizView.vue";
import Results from "@/views/Results/Results.vue";

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
      this.formValues = values;
      this.activeView = "quiz";
    },
    saveQuestionAnswer(value: string) {
      this.questionAnswers.push(value);
    },
    goToResults() {
      this.activeView = "results";
    },
    retakeQuiuz() {
      this.activeView = "homepage";
      this.questionAnswers = [];
    },
  },
});
</script>

<style lang="scss">
@import "./Quiz";
</style>
