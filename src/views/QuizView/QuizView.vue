<template>
  <div v-if="!loading">
    <h2>{{ quizQuestions[activeQuestionIndex].title }}</h2>
    <ul>
      <li
        v-for="answer of activeQuestionAnswers"
        :key="answer.id"
        :value="answer.id"
      >
        {{ answer.title }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import { Quiz } from "@/pages/Quiz.vue";

export default defineComponent({
  name: "QuizView",
  props: ["quizId"],
  data: () => ({
    loading: true,
    quizQuestions: [] as Quiz[],
    activeQuestionIndex: 0,
    activeQuestionAnswers: [] as Quiz[],
  }),
  async created() {
    await axios
      .get(
        `https://printful.com/test-quiz.php?action=questions&quizId=${this.quizId}`
      )
      .then(({ data }) => {
        this.quizQuestions = data;
      });
    const questionId = this.quizQuestions[this.activeQuestionIndex].id;
    axios
      .get(
        `https://printful.com/test-quiz.php?action=answers&quizId=${this.quizId}&questionId=${questionId}`
      )
      .then(({ data }) => {
        this.activeQuestionAnswers = data;
        this.loading = false;
      });
  },
  computed: {
    numberOfQuestions() {
      return this.quizQuestions.length;
    },
  },
  watch: {
    activeQuestionIndex() {
      this.loading = true;
      const questionId = this.quizQuestions[this.activeQuestionIndex].id;
      axios
        .get(
          `https://printful.com/test-quiz.php?action=answers&quizId=${this.quizId}&questionId=${questionId}`
        )
        .then(({ data }) => {
          this.activeQuestionAnswers = data;
          this.loading = false;
        });
    },
  },
});
</script>

<style lang="scss">
@import "./QuizView";
</style>
