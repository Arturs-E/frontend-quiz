<template>
  <div>
    <Heading3 class="quiz-question">{{ getQuestion() }}</Heading3>
    <form @submit.prevent="submitAnswer" class="quiz">
      <div class="answer-container-wrapper">
        <FetchingError v-if="fetchingError" />
        <Loader v-else-if="loading" />
        <div v-else class="answer-container">
          <QuizAnswer
            v-for="answer of activeQuestionAnswers"
            :key="answer.id"
            :id="answer.id"
            :title="answer.title"
            @update:model-value="selectedAnswer = $event"
          />
        </div>
      </div>
      <Button variant="primary" :disabled="!selectedAnswer">
        {{ checkIfLastQuestion() ? "Submit" : "Next question" }}
      </Button>
    </form>
    <ProgressBar
      v-if="numberOfQuestions"
      :width="getProgressBarCompleteness()"
      :content="getProgressBarContent()"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import { Quiz } from "@/pages/Quiz.vue";
import QuizAnswer from "@/components/quiz-answer/QuizAnswer.vue";
import Button from "@/components/buttons/Button.vue";
import ProgressBar from "@/components/progress-bar/ProgressBar.vue";
import Loader from "@/components/loader/Loader.vue";
import FetchingError from "@/components/fetching-error/FetchingError.vue";
import Heading3 from "@/components/headings/Heading3.vue";

export default defineComponent({
  name: "QuizTest",
  components: {
    QuizAnswer,
    Button,
    ProgressBar,
    Loader,
    FetchingError,
    Heading3,
  },
  props: ["quizId"],
  emits: ["onSubmitAnswerHandler", "onLastQuestion"],
  data: () => ({
    fetchingError: false,
    loading: true,
    quizQuestions: [] as Quiz[],
    activeQuestionIndex: 0,
    activeQuestionAnswers: [] as Quiz[],
    selectedAnswer: "",
  }),
  async created() {
    await axios
      .get("/test-quiz.php", {
        params: {
          action: "questions",
          quizId: this.quizId,
        },
      })
      .then(({ data }) => {
        this.quizQuestions = data;
      })
      .catch((error) => {
        if (error) {
          this.fetchingError = true;
        }
      });

    if (this.fetchingError) {
      return;
    }
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
        .get("/test-quiz.php", {
          params: {
            action: "answers",
            quizId: this.quizId,
            questionId,
          },
        })
        .then(({ data }) => {
          this.activeQuestionAnswers = data;
          this.loading = false;
        })
        .catch((error) => {
          if (error) {
            this.fetchingError = true;
          }
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
@import "QuizTest";
</style>
