<template>
  <div id="app">
    <div id="nav">
      <router-link to="/" class="nav-link">Home</router-link>
      <router-link to="/" class="nav-link">Favourites</router-link>
    </div>
    <router-view />
  </div>
</template>

<style>
body {
  font-family: "Quicksand", sans-serif;
  font-size: 18px;
  margin: 0 0 0 250px;
}

#nav {
  position: fixed;
  left: 0;
  top: 0;
  background: #222222;
  min-height: 100vh;
  width: 250px;
  padding-top: 50px;
}

.nav-link {
  color: #eeeeee;
  display: block;
  padding: 20px 22px;
  text-decoration: none;
}

.nav-link:hover {
  background: rgba(255, 255, 255, 0.05);
}
</style>
