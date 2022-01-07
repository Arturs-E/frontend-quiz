<template>
  <div v-if="!loading">
    <h2>{{ `Thanks, ${formValues.name}!` }}</h2>
    <span>
      {{
        `You answered correctly to ${quizResults.correct} out of ${quizResults.total} questions.`
      }}
    </span>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import axios from "axios";
import { FormValues } from "@/views/Homepage/Homepage.vue";

type QuizResults = {
  correct: number;
  total: number;
};

export default defineComponent({
  name: "Results",
  props: {
    formValues: Object as PropType<FormValues>,
    answers: Array as PropType<string[]>,
  },
  data: () => ({
    loading: true,
    quizResults: {} as QuizResults,
  }),
  created() {
    const quizId = this.formValues?.quizId;
    const answers = `&answers[]=${this.answers?.join("&answers[]=")}`;
    axios
      .get(
        `https://printful.com/test-quiz.php?action=submit&quizId=${quizId}${answers}`
      )
      .then(({ data }) => {
        this.quizResults = data;
        this.loading = false;
      });
  },
});
</script>

<style lang="scss">
@import "Results";
</style>
