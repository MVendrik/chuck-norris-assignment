<template>
  <div class="joke">
    <span v-on:click="addToFavourites" ref="text">{{ text }}</span>
  </div>
</template>

<script>
export default {
  name: "Joke",
  props: ["text"],
  methods: {
    addToFavourites: function() {
      let existingFavourites = JSON.parse(localStorage.getItem("favourites"));
      console.log(typeof existingFavourites);

      if (existingFavourites == null) {
        existingFavourites = [];
        console.log(existingFavourites);
      }
      let jokeToAdd = this.$refs.text.innerText;

      existingFavourites.push(jokeToAdd);
      console.log(existingFavourites);
      window.localStorage.setItem(
        "favourites",
        JSON.stringify(existingFavourites)
      );
    },
  },
};
</script>

<style>
.joke {
  border-top: 1px solid #222222;
  padding: 20px 0;
}
</style>
