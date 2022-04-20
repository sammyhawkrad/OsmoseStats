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
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background-color: #f3eff5;
}
</style>
