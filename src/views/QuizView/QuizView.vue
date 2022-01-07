<template>
  <div v-if="!loading">
    <h2>{{ quizQuestions[activeQuestionIndex].title }}</h2>
    <form @submit.prevent="submitHandler">
      <div class="answer-container">
        <div
          class="answer-wrapper"
          v-for="answer of activeQuestionAnswers"
          :key="answer.id"
        >
          <input
            type="radio"
            name="answers"
            class="btn-check"
            :id="answer.id"
            :value="answer.id"
            v-model="selectedAnswer"
          />
          <label
            class="btn btn-outline-secondary quiz-answer-label"
            :for="answer.id"
          >
            {{ answer.title }}
          </label>
        </div>
      </div>
      <button type="submit" class="btn btn-primary">Next</button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import { Quiz } from "@/pages/Quiz.vue";

export default defineComponent({
  name: "QuizView",
  props: ["quizId"],
  emits: ["onSubmitAnswerHandler", "onLastQuestion"],
  data: () => ({
    loading: true,
    quizQuestions: [] as Quiz[],
    activeQuestionIndex: 0,
    activeQuestionAnswers: [] as Quiz[],
    selectedAnswer: "",
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
  methods: {
    submitHandler() {
      this.$emit("onSubmitAnswerHandler", this.selectedAnswer);

      if (this.activeQuestionIndex + 1 === this.numberOfQuestions) {
        this.$emit("onLastQuestion");
        return;
      }

      this.activeQuestionIndex++;
    },
  },
});
</script>

<style lang="scss">
@import "./QuizView";
</style>
