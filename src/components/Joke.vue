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

      if (existingFavourites == null) {
        existingFavourites = [];
      }

      let jokeToAdd = this.$refs.text.innerText;
      existingFavourites.push(jokeToAdd);

      // Remove if entry if it is already a favorite
      existingFavourites = existingFavourites.filter((elem, index, self) => {
        return index === self.indexOf(elem);
      });

      // Only 10 items allowed. If more than 10 items, remove the first one added
      if (existingFavourites.length > 10) {
        existingFavourites.shift();
      }

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
