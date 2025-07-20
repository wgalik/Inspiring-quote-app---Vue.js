<template>
  <div class="container mt-5">
    <p class="text-center">
      We didn't find a quote that matches your search, but here's our suggestion
    </p>
    <hr class="border border-1 border-dark w-75 mx-auto" />
    <blockquote class="blockquote text-end ms-5 me-5 mt-5 fs-1">
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
  data() {
    return { quote: "", author: "" };
  },
  methods: {
    getRandomQuote() {
      fetch(`https://api.quotable.io/quotes/random`)
        .then((res) => res.json())
        .then((res) => {
          this.quote = res[0].content;
          this.author = res[0].author;
        });
    },
    backToStart() {
      this.$emit("goto", 0);
    },
  },
  mounted() {
    this.getRandomQuote();
  },
};
</script>

<style></style>
