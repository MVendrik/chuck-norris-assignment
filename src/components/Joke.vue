<template>
  <div class="joke">
    <span>{{ text }}</span>
  </div>
</template>

<script>
export default {
  name: "Joke",
  props: ["text"],
};
</script>

<style>
.joke {
  border-top: 1px solid #222222;
  padding: 20px 0;
}
</style>
