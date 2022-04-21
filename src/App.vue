<template>
  <div>
    <header>
      <NavBar />
    </header>
    <main>
      <router-view />
    </main>
  </div>
</template>

<script>
import NavBar from "./components/NavBar.vue";

export default {
  name: "App",
  components: { NavBar },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=PT+Sans:ital@0;1&display=swap");

:root {
  font-family: "PT Sans", sans-serif;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background-color: #e9e9e9;
}
</style>
