<template>
  <div class="content-container">
    <div class="content-wrapper">
      <Homepage v-if="activeView === 'homepage'" @onSubmit="setFormValues" />
      <QuizView v-else-if="activeView === 'quiz'" :quizId="formValues.id" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Homepage from "@/views/Homepage/Homepage.vue";
import QuizView from "@/views/QuizView/QuizView.vue";

export type Quiz = {
  id: string;
  title: string;
};

export default defineComponent({
  name: "Quiz",
  components: {
    QuizView,
    Homepage,
  },
  data: () => ({
    activeView: "homepage" as "homepage" | "quiz" | "results",
    formValues: {} as Quiz,
  }),
  methods: {
    setFormValues(values: Quiz) {
      this.formValues = values;
      console.log(this.formValues);
      this.activeView = "quiz";
    },
  },
});
</script>

<style lang="scss">
@import "./Quiz";
</style>
