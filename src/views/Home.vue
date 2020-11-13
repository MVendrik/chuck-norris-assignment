<template>
  <div class="home">
    <button class="joke-button" v-on:click="generateJokes">
      Generate 10 jokes
    </button>
  </div>
</template>

<script>
export default {
  name: "Home",
  props: {},
  methods: {
    generateJokes: async function() {
      let fetchedData = [];

      let response = await fetch("http://api.icndb.com/jokes/random/10");

      let parsed = await response.json();

      fetchedData = parsed.value.map((item) => item.joke);

      console.log(fetchedData);
    },
  },
};
</script>
