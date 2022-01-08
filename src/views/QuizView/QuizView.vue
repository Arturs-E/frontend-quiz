<template>
  <div>
    <h2 class="mb-3">{{ quizQuestions[activeQuestionIndex]?.title }}</h2>
    <form @submit.prevent="submitHandler">
      <div class="answer-container-wrapper">
        <div class="answer-container" v-if="!loading">
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
      </div>
      <button
        type="submit"
        class="btn btn-primary"
        :style="selectedAnswer ? '' : { opacity: 0.4 }"
      >
        Next
      </button>
    </form>
    <div class="progress-bar-outer">
      <div
        class="progress-bar-inner"
        :style="{ width: `${getProgressBarCompleteness()}%` }"
      >
        {{ `${activeQuestionIndex + 1} / ${numberOfQuestions}` }}
      </div>
    </div>
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
      .get("", {
        params: {
          action: "questions",
          quizId: this.quizId,
        },
      })
      .then(({ data }) => {
        this.quizQuestions = data;
      });
    this.getAnswers();
  },
  computed: {
    numberOfQuestions() {
      return this.quizQuestions.length;
    },
  },
  watch: {
    activeQuestionIndex() {
      this.loading = true;
      this.getAnswers();
    },
  },
  methods: {
    getAnswers() {
      const questionId = this.quizQuestions[this.activeQuestionIndex].id;
      axios
        .get("", {
          params: {
            action: "answers",
            quizId: this.quizId,
            questionId,
          },
        })
        .then(({ data }) => {
          this.activeQuestionAnswers = data;
          this.loading = false;
        });
    },
    submitHandler() {
      if (!this.selectedAnswer) {
        return;
      }
      this.$emit("onSubmitAnswerHandler", this.selectedAnswer);

      if (this.activeQuestionIndex + 1 === this.numberOfQuestions) {
        this.$emit("onLastQuestion");
        return;
      }

      this.activeQuestionIndex++;
      this.selectedAnswer = "";
    },
    getProgressBarCompleteness() {
      return Math.round(
        ((this.activeQuestionIndex + 1) / this.numberOfQuestions) * 100
      );
    },
  },
});
</script>

<style lang="scss">
@import "./QuizView";
</style>
