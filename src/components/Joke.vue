<template>
  <div class="joke">
    <span ref="text" class="joke-text">{{ text }} </span>
    <span
      v-on:click="addToFavourites"
      class="favourite-button"
      v-bind:class="{ active: !isFavourite }"
    >
      favourite</span
    >
    <span
      v-on:click="deleteFavourite"
      class="delete-button"
      v-bind:class="{ active: isFavourite }"
    >
      delete</span
    >
  </div>
</template>

<script>
export default {
  name: "Joke",
  props: {
    text: String,
    isFavourite: Boolean,
  },
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

      this.isFavourite = true;

      window.localStorage.setItem(
        "favourites",
        JSON.stringify(existingFavourites)
      );
    },
    deleteFavourite: function() {
      let existingFavourites = JSON.parse(localStorage.getItem("favourites"));

      existingFavourites = existingFavourites.filter(
        (item) => item !== this.$refs.text.innerText
      );

      window.localStorage.setItem(
        "favourites",
        JSON.stringify(existingFavourites)
      );

      this.isFavourite = false;

      this.$emit("delete-Favourite", this.$refs.text.innerText);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Goldman&display=swap");

.joke {
  border-top: 1px solid #222222;
  padding: 20px 0;
}

.joke:hover {
  background: #eeeeee;
}

.joke-text {
  display: block;
  margin-bottom: 15px;
  font-family: "Goldman", cursive;
  font-weight: bold;
  font-size: 18px;
}

.favourite-button,
.delete-button {
  display: none;
  cursor: pointer;
  padding: 0 15px;
  background-color: #222222;
  border-radius: 16px;
  color: #eeeeee;
  height: 32px;
  align-items: center;
}

.active {
  display: inline-flex;
}
</style>
