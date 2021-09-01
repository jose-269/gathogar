<template>
  <div class="load">
    <v-progress-circular
      class="load__progress"
      indeterminate
      color="purple"
      size="70"
      width="7"
    ></v-progress-circular>
  </div>
</template>

<script>
export default {
  name: "Spinner",
};
</script>
<style lang="scss" scoped>
.load {
    text-align: center;
    height: 50vh;
    width: 100%;
    &__progress {
        margin-top: 25vh;
        // height: 50%;
    }
}
</style>
