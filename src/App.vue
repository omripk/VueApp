<template>
  <div id="app">
    <search v-on:SearchRequested="handleSearch"></search>
    <p v-if="isLoading">Loading</p>
    <preview :gifs="gifs"></preview>
  </div>
</template>

<script>
import Search from "./components/Search.vue";
import Preview from "./components/Preview.vue";

export default {
  name: "app",
  components: { Search, Preview },
  data() {
    return {
      isLoading: true,
      gifs: []
    };
  },
  // Uygulama ilk açıldığında
  created() {
    const url =
      "https://api.giphy.com/v1/gifs/trending?api_key=VRMP3ZgWCkGmJhHQjx90VpDyeNYIrKWl&limit=10&rating=G";
    this.doQuery(url);
  },
  methods: {
    doQuery(url) {
      fetch(url)
        .then(res => {
          return res.json();
        })
        .then(res => {
          this.gifs = res.data;
          this.isLoading = false;
        });
    },
    handleSearch(query) {
      this.gifs = [];
      this.isLoading = true;

      const url = `https://api.giphy.com/v1/gifs/search?api_key=VRMP3ZgWCkGmJhHQjx90VpDyeNYIrKWl&q=${query}&limit=25&offset=0&rating=G&lang=en`;
      this.doQuery(url);
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>