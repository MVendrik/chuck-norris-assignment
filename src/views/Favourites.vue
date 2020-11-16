<template>
  <div class="favourites">
    <h1>Favourites</h1>
    <button
      class="random-button"
      v-on:click="addRandomFavourites"
      v-bind:class="{
        disabled: disableButton,
        addingActive: isAddingFavourites,
      }"
    >
      Add random favourites
    </button>
    <span class="disclaimer">There is a maximum of 10 favourites</span>
    <div ref="container">
      <Joke
        v-for="(text, index) in currentFavourites"
        :key="index"
        :text="text"
        :isFavourite="true"
        @delete-Favourite="deleteFavourite"
      />
    </div>
  </div>
</template>

<script>
import Joke from "@/components/Joke.vue";

export default {
  name: "Favourites",
  components: {
    Joke,
  },
  data() {
    return {
      currentFavourites: [],
      disableButton: false,
      isAddingFavourites: false,
    };
  },
  methods: {
    addRandomFavourites: function() {
      let existingFavourites = JSON.parse(localStorage.getItem("favourites"));

      if (existingFavourites >= 9) {
        // Do nothing if there are already 10 favourites.
        return;
      }

      this.isAddingFavourites = !this.isAddingFavourites;
      console.log(this.isAddingFavourites);

      let timer = setInterval(
        async function() {
          if (!this.isAddingFavourites) {
            clearInterval(timer);
            console.log("stopped");
          } else {
            let response = await fetch("http://api.icndb.com/jokes/random/1");
            let parsed = await response.json();

            let randomJoke = parsed.value.map((item) => item.joke);

            let existingFavourites = JSON.parse(
              localStorage.getItem("favourites")
            );

            if (existingFavourites == null) {
              existingFavourites = [];
            }

            if (existingFavourites.length >= 9) {
              clearInterval(timer);
              this.disableButton = true;
              this.isAddingFavourites = false;
            }

            existingFavourites.push(randomJoke[0]);

            window.localStorage.setItem(
              "favourites",
              JSON.stringify(existingFavourites)
            );

            this.currentFavourites = existingFavourites;
            this.$forceUpdate();
          }
        }.bind(this),
        5000
      );
    },
    deleteFavourite: function(e) {
      this.currentFavourites = this.currentFavourites.filter(
        (item) => item !== e
      );

      if (this.currentFavourites.length < 10) {
        this.disableButton = false;
      }

      this.$forceUpdate();
    },
  },
  mounted() {
    let favourites = JSON.parse(localStorage.getItem("favourites"));

    this.currentFavourites = favourites;

    if (this.currentFavourites.length >= 9) {
      this.disableButton = true;
    }
  },
};
</script>

<style>
.favourites {
  margin: 0 2em;
}

.favourites h1 {
  text-transform: uppercase;
  margin: 45px 0;
}

.random-button {
  background-color: #eeeeee;
  border: 2px solid #222222;
  padding: 20px 22px;
  cursor: pointer;
  text-transform: uppercase;
  margin-bottom: 5px;
}

.disclaimer {
  font-size: 12px;
  display: block;
  margin-bottom: 45px;
}

.disabled {
  pointer-events: none;
  opacity: 0.4;
}

.addingActive {
  background-color: green;
}

.random-button:hover {
  background-color: #222222;
  color: #eeeeee;
}
</style>
