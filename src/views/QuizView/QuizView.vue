<template>
  <div>
    <h3 class="mb-3">{{ getQuestion() }}</h3>
    <form @submit.prevent="submitAnswer" class="quiz">
      <div class="answer-container-wrapper">
        <div class="answer-container" v-if="!loading">
          <QuizAnswer
            v-for="answer of activeQuestionAnswers"
            :key="answer.id"
            :id="answer.id"
            :title="answer.title"
            @update:model-value="selectedAnswer = $event"
          />
        </div>
      </div>
      <button
        type="submit"
        class="btn btn-primary"
        :style="selectedAnswer ? '' : { opacity: 0.4 }"
        :disabled="!selectedAnswer"
      >
        {{ checkIfLastQuestion() ? "Submit" : "Next" }}
      </button>
    </form>
    <div v-if="numberOfQuestions" class="progress-bar-outer">
      <div
        class="progress-bar-inner"
        :style="{ width: `${getProgressBarCompleteness()}%` }"
      >
        {{ getProgressBarContent() }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import { Quiz } from "@/pages/Quiz.vue";
import QuizAnswer from "@/components/quiz-answer/QuizAnswer.vue";

export default defineComponent({
  name: "QuizView",
  components: {
    QuizAnswer,
  },
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
    getQuestion() {
      return this.quizQuestions[this.activeQuestionIndex]?.title;
    },
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
    submitAnswer() {
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
    getProgressBarContent() {
      return `${this.activeQuestionIndex + 1} / ${this.numberOfQuestions}`;
    },
    checkIfLastQuestion() {
      return this.activeQuestionIndex + 1 === this.numberOfQuestions;
    },
  },
});
</script>

<style lang="scss">
@import "./QuizView";
</style>
