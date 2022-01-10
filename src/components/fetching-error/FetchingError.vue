<template>
  <Heading4 :alignment="'centered'">Could not retrieve data...</Heading4>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Heading4 from "@/components/headings/Heading4.vue";

export default defineComponent({
  name: "FetchingError",
  components: {
    Heading4,
  },

});
</script>
