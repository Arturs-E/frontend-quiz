<template>
  <div v-if="!loading" class="results">
    <h2 style="text-transform: capitalize">
      {{ `Thanks, ${formValues.name}!` }}
    </h2>
    <span class="mb-4">
      {{
        `You answered correctly to ${quizResults.correct} out of ${quizResults.total} questions.`
      }}
    </span>
    <button
      type="button"
      class="btn btn-primary"
      @click="this.$emit('onClick')"
    >
      Re-take
    </button>
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
@import "Results";
</style>
