<template>
  <div>
    <SearchBar
      @emitSearch="getSearch"
      :username="username"
      v-model="username"
    />
    <div id="search-results" v-if="searchResults.length > 0">
      <li
        class="list-item"
        v-for="user in searchResults"
        :key="user.id"
        @click="selectUser(user)"
      >
        {{ user.names[0] }}
      </li>
    </div>
    {{ username }}
    {{ searchResults }}
    <UserProfile
      v-if="userSelected"
      :selectedUser="selectedUser"
      :osmProfile="osmProfile"
    />
  </div>
</template>

<script>
import axios from "axios";
// import router from "../router";
import SearchBar from "../components/SearchBar.vue";
import UserProfile from "../components/UserProfile.vue";

export default {
  name: "HomePage",
  components: { SearchBar, UserProfile },
  data() {
    return {
      username: "",
      selectedUser: {},
      searchResults: [],
      userSelected: false,
      osmProfile: {},
    };
  },
  methods: {
    async getSearch(event) {
      this.username = event;
      try {
        let whosthat = await axios.get(
          `http://whosthat.osmz.ru/whosthat.php?action=names&name=${event}`
        );
        this.searchResults = whosthat.data;
      } catch (error) {
        console.log(error);
      }
    },
    selectUser(user) {
      Object.assign(this.selectedUser, user);
      console.log(this.selectedUser);
      this.username = null;
      this.searchResults = [];
      this.userSelected = true;
      this.getOsmProfile();
    },
    async getOsmProfile() {
      try {
        const { user } = (
          await axios.get(
            `https://api.openstreetmap.org/api/0.6/user/${this.selectedUser.id}.json`
          )
        ).data;
        this.osmProfile = user;
        console.log(user);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style scoped>
#search-results {
  position: relative;
  top: 80px;
  background-color: white;
  width: 350px;
  border-radius: 5px;
  margin: auto;
}

.list-item {
  list-style-type: none;
  padding: 7px 10px;
  border-bottom: 1px solid #f3eff5;
}

.list-item:hover {
  background: rgb(245, 237, 163);
}

a:hover {
  color: burlywood;
}
</style>
