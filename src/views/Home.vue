<template>
  <div class="home">
    <h1 class="header">Chuck Norris <s>jokes</s> <span>facts!</span></h1>
    <button class="joke-button" v-on:click="generateJokes">
      Generate jokes
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

<style>
.home {
  margin: 0 2em;
}

.header {
  text-transform: uppercase;
  margin: 45px 0;
}

.header span {
  display: inline-block;
  transform: rotate(-12deg);
}

.joke-button {
  background-color: #eeeeee;
  border: 2px solid #222222;
  padding: 20px 22px;
  cursor: pointer;
  text-transform: uppercase;
  margin-bottom: 45px;
}

.joke-button:hover {
  background-color: #222222;
  color: #eeeeee;
}
</style>
