<template>
  <div class="joke">
    <p>{{ text }}</p>
  </div>
</template>

<script>
export default {
  name: "Joke",
  props: ["text"],
};
</script>
