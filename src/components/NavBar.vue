<template>
  <nav>
    <div id="logo">
      <img src="https://osmose.openstreetmap.fr/favicon.png" alt="" />
    </div>
    <h1><a href="/">Osmose Statistics</a></h1>
  </nav>
</template>

<script>
export default {
  name: "NavBar",
};
</script>

<style>
nav {
  position: sticky;
  background-color: #ffd449;
  opacity: 75%;
  width: 100%;
  padding: 1.5rem;
  box-shadow: 1px 1px 5px lightgrey;
  display: grid;
  grid-template-columns: max-content 1fr max-content;
  column-gap: 1rem;
}

#logo > img {
  object-fit: cover;
  height: 1.5rem;
  width: 1.5rem;
}

h1 > a:visited,
a:link {
  text-decoration: none;
  color: black;
}
</style>
