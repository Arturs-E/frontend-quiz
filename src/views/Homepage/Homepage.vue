<template>
  <form class="form">
    <div class="form__input-wrapper">
      <div>
        <label for="name" class="form-label">Name</label>
        <input
          type="email"
          class="form-control"
          id="name"
          placeholder="Please enter your name"
          aria-describedby="nameHelp"
        />
        <div id="nameHelp" class="form-text">
          We'll never share your name with anyone else.
        </div>
      </div>
      <select class="form-select" aria-label="Default select example">
        <option selected hidden disabled>Please choose test</option>
        <option value="1">One</option>
        <option value="2">Two</option>
        <option value="3">Three</option>
      </select>
    </div>
    <div class="form__submit-wrapper">
      <button type="submit" class="btn btn-primary">Start</button>
    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Homepage",
});
</script>

<style lang="scss">
@import "./Homepage";
</style>
