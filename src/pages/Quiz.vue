<template>
  <div class="content-container">
    <div class="content-wrapper">
      <Homepage v-if="activeView === 'homepage'" @onSubmit="setFormValues" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Homepage, { QuizOptions } from "@/views/Homepage/Homepage.vue";

export default defineComponent({
  name: "Quiz",
  components: {
    Homepage,
  },
  data: () => ({
    activeView: "homepage" as "homepage" | "quiz" | "results",
    formValues: {} as QuizOptions,
  }),
  methods: {
    setFormValues(values: QuizOptions) {
      this.formValues = values;
      this.activeView = "quiz";
    },
  },
});
</script>

<style lang="scss">
@import "./Quiz";
</style>
