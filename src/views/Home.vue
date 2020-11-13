<template>
  <div class="home">
    <button class="joke-button" v-on:click="generateJokes">
      Generate 10 jokes
    </button>
    <div ref="container"></div>
  </div>
</template>

<script>
import Vue from "vue";
import Joke from "@/components/Joke.vue";

export default {
  name: "Home",
  props: {},
  methods: {
    generateJokes: async function() {
      let fetchedData = [];

      let response = await fetch("http://api.icndb.com/jokes/random/10");

      let parsed = await response.json();

      fetchedData = parsed.value.map((item) => item.joke);

      // To remove the old jokes, if there are any
      while (this.$refs.container.firstChild) {
        this.$refs.container.removeChild(this.$refs.container.lastChild);
      }

      for (let item of fetchedData) {
        const ComponentClass = Vue.extend(Joke);
        let instance = new ComponentClass({
          propsData: { text: item },
        });

        instance.$mount();
        this.$refs.container.appendChild(instance.$el);
      }
    },
  },
};
</script>
