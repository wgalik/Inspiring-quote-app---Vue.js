<template>
  <div class="container mt-5">
    <h1 class="text-center fs-1">Find an inspiring quote</h1>

    <form class="d-flex flex-column align-items-center mt-3">
      <label for="tag" class="form-label fs-5">Find by author or tag</label>
      <hr class="border border-1 border-dark w-50" />
      <input
        name="tag"
        type="text"
        class="form-control mb-2 w-50"
        id="tag"
        v-model="tag"
        placeholder="write: author's 'name' or 'tag'" />
      <button
        type="submit"
        class="btn w-50"
        :="{ disabled: !tag }"
        @:click.prevent="nextScreen"
        @:keyup.enter.prevent="nextScreen">
        Find
      </button>
    </form>
    <p
      class="position-fixed mt-5 p-5 top-0 start-50 translate-middle-x translate-middle-x z-3"
      v-html="waitingSpinner"></p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tag: "",
      quote: "",
      author: "",
      waitingSpinner: "",
    };
  },
  methods: {
    nextScreen() {
      this.waitingSpinner = `<div class="spinner-border m-5" role="status">
    <span class="visually-hidden">Loading...</span>
  </div>`;
      fetch(`https://api.quotable.io/quotes?tags=${this.tag}`)
        .then((res) => res.json())
        .then((res) => {
          if (!res.results.length) {
            this.getAuthor();
          } else {
            let index = Math.floor(Math.random() * res.results.length);

            this.quote = res.results[index].content;
            this.author = res.results[index].author;
            this.$emit("goto", 1);
            this.$emit("quote", this.quote);
            this.$emit("author", this.author);
          }
        });
    },

    getAuthor() {
      let authorSlug;
      fetch(`https://api.quotable.io/search/authors?query=${this.tag}`)
        .then((res) => res.json())
        .then((res) => {
          let index = Math.floor(Math.random() * res.results.length);

          if (!res.results.length) {
            this.$emit("goto", 2);
          } else if (!res.results[index].quoteCount) {
            this.getAuthor();
          } else {
            authorSlug = res.results[index].slug;
            this.getData(authorSlug);
          }
        });
    },

    getData(authorSlug) {
      fetch(`https://api.quotable.io/quotes?author=${authorSlug}`)
        .then((res) => res.json())
        .then((res) => {
          let index = Math.floor(Math.random() * res.results.length);
          this.quote = res.results[index].content;
          this.author = res.results[index].author;
          this.$emit("goto", 1);
          this.$emit("quote", this.quote);
          this.$emit("author", this.author);
        });
    },
  },
};
</script>

<style></style>
