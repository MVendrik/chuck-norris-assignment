<template>
  <div class="favourites">
    <h1>Favourites</h1>
    <button class="random-button" v-on:click="addRandomFavourites">
      Add random favourites
    </button>
    <div ref="container"></div>
  </div>
</template>

<script>
import Joke from "@/components/Joke.vue";
import Vue from "vue";

export default {
  name: "Favourites",
  methods: {
    addRandomFavourites: function() {
      let existingFavourites = JSON.parse(localStorage.getItem("favourites"));

      if (existingFavourites >= 10) {
        // Do nothing if there are already 10 favourites.
        return;
      }

      setInterval(async function() {
        let response = await fetch("http://api.icndb.com/jokes/random/1");
        let parsed = await response.json();

        let randomJoke = parsed.value.map((item) => item.joke);
        console.log(randomJoke);

        let existingFavourites = JSON.parse(localStorage.getItem("favourites"));

        if (existingFavourites == null) {
          existingFavourites = [];
        }

        existingFavourites.push(randomJoke[0]);

        window.localStorage.setItem(
          "favourites",
          JSON.stringify(existingFavourites)
        );
      }, 5000);
    },
  },
  mounted() {
    let favourites = JSON.parse(localStorage.getItem("favourites"));

    for (let favourite of favourites) {
      const ComponentClass = Vue.extend(Joke);
      let instance = new ComponentClass({
        propsData: { text: favourite, isFavourite: true },
      });

      instance.$mount();
      this.$refs.container.appendChild(instance.$el);
    }
  },
};
</script>

<style>
.favourites {
  margin: 0 2em;
}

.random-button {
  background-color: #eeeeee;
  border: 2px solid #222222;
  padding: 20px 22px;
  cursor: pointer;
  text-transform: uppercase;
  margin-bottom: 2em;
}
</style>
