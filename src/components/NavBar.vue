<template>
  <nav>
    <div id="logo">
      <img src="https://osmose.openstreetmap.fr/favicon.png" alt="" />
    </div>
    <h1>Osmose Statistics</h1>
    <form>
      <input
        type="search"
        name=""
        id="search-bar"
        placeholder="Enter OSM username"
      />
      <input type="submit" id="search-button" value="Search" />
    </form>
  </nav>
</template>

<script setup></script>

<style scoped>
nav {
  position: sticky;
  background-color: #ffd449;
  opacity: 75%;
  width: 100%;
  padding: 1.5rem;
  box-shadow: 1px 1px 5px lightgrey;
  display: grid;
  grid-template-columns: 0fr 1fr max-content;
  column-gap: 1rem;
}

#logo > img {
  object-fit: cover;
  height: 1.5rem;
  width: 1.5rem;
}

#search-bar {
  padding: 0.7rem;
  border: none;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
  width: 300px;
}

#search-button {
  padding: 0.7rem;
  border: none;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
  background-color: black;
  color: white;
}
</style>
