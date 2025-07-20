<template>
  <div class="container mt-5">
    <blockquote class="blockquote text-end ms-5 me-5 fs-1">
      {{ quote }}
    </blockquote>
    <figcaption class="blockquote-footer text-end mt-3 me-5 fs-5">
      {{ author }}
    </figcaption>
    <button class="btn mx-auto mt-5 mb-5 d-block" @:click="backToStart">
      Find new one
    </button>
  </div>
</template>
<script>
export default {
  props: ["quote", "author"],
  methods: {
    backToStart() {
      this.$emit("goto", 0);
    },
  },
};
</script>

<style></style>
