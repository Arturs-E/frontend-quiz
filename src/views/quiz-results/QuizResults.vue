<template>
  <div v-if="!loading" class="results">
    <h2 style="text-transform: capitalize">
      {{ `Thanks, ${formValues.name}!` }}
    </h2>
    <span class="results__text">
      {{
        `You answered correctly to ${quizResults.correct} out of ${quizResults.total} questions.`
      }}
    </span>
    <Button additional-class="primary" @button:on-click="this.$emit('onClick')">
      Re-take quiz
    </Button>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import axios from "axios";
import Button from "@/components/buttons/Button.vue";
import { FormValues } from "@/views/quiz-form/QuizForm.vue";

type QuizResults = {
  correct: number;
  total: number;
};

export default defineComponent({
  name: "QuizResults",
  components: {
    Button,
  },

  emits: ["onClick"],
  props: {
    formValues: {
      type: Object as PropType<FormValues>,
      required: true,
    },
    answers: {
      type: Array as PropType<string[]>,
      required: true,
    },
  },
  data: () => ({
    loading: true,
    quizResults: {} as QuizResults,
  }),
  created() {
    axios
      .get("", {
        params: {
          action: "submit",
          quizId: this.formValues.quizId,
          answers: this.answers,
        },
      })
      .then(({ data }) => {
        this.quizResults = data;
        this.loading = false;
      });
  },
});
</script>

<style lang="scss">
@import "QuizResults";
</style>
