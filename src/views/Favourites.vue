<template>
  <div class="favourites">
    <h1>Favourites</h1>
    <div ref="container"></div>
  </div>
</template>

<script>
import Joke from "@/components/Joke.vue";
import Vue from "vue";

export default {
  name: "Favourites",
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
</style>
