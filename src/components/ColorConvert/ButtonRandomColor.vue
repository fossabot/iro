<template>
  <div class="section random-wrapper">
    <div class="label"><span class="heading">Generate</span> random color:</div>
    <div class="content">
      <button class="button" @click="onRandomColor">RANDOM COLOR</button>
    </div>
  </div>
</template>

<script>
import { generateRandomColor } from "@src/services/colors";

export default {
  name: "ButtonRandomColor",
  methods: {
    onRandomColor() {
      this.$emit("colorChanged", ["rgb", generateRandomColor()]);
    },
  }
};
</script>

<style></style>
