<template>
  <div class="color-toggle">
    <input type="checkbox" name="" id="color-toggle" />
    <label for="color-toggle" @:click="colorMode"></label>
  </div>

  <transition name="fade" appear mode="out-in">
    <component
      :is="screens[position]"
      @goto="nextScreen"
      :quote="quote"
      @quote="getQuote"
      :author="author"
      @author="getAuthor"
  /></transition>
</template>

<script>
import start from "./components/start-component.vue";
import sentence from "./components/sentence-component.vue";
import empty from "./components/empty-component.vue";

export default {
  components: {
    start,
    sentence,
    empty,
  },

  data() {
    return {
      position: 0,
      screens: ["start", "sentence", "empty"],
      quote: "",
      author: "",
      dark_mode: false,
    };
  },
  methods: {
    nextScreen(position) {
      this.position = position;
    },

    getQuote(quote) {
      this.quote = quote;
    },
    getAuthor(author) {
      this.author = author;
    },
    colorMode() {
      const container = document.querySelector(".container");
      document.body.classList.toggle("dark-mode");
      container.classList.toggle("dark-mode");

      this.dark_mode = !this.dark_mode;
    },
  },
};
</script>

<style>
@import "./assets/style.scss";
</style>
