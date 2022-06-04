<template>
  <div>
    <form @submit.prevent>
      <input
        autocomplete="off"
        type="text"
        id="search-bar"
        placeholder="Enter OSM username"
        name="username"
        @change="emitSearch"
      />
      <button type="submit" id="search-button">Search</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  props: { username: String },
  methods: {
    emitSearch(event) {
      this.$emit("emitSearch", event.target.value);
    },
  },
};
</script>

<style scoped>
form {
  width: max-content;
  margin: 10rem auto 2rem auto;
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
