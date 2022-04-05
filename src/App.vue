<template>
  <div>
    <header>
      <NavBar />
    </header>
    <main>
      <UserProfile />
      <UserStats />
    </main>
  </div>
</template>

<script setup>
import NavBar from "./components/NavBar.vue";
import UserProfile from "./components/UserProfile.vue";
import UserStats from "./components/UserStats.vue";
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
