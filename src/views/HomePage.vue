<template>
  <div id="homepage">
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
    <div v-if="userSelected">
      <div class="summary">
        <UserProfile :selectedUser="selectedUser" :osmProfile="osmProfile" />
        <OsmoseOverview :overview="overview" />
      </div>
      <div id="levels">
        <div class="component" v-if="overview[1] > 0">
          <h3>Level 1 (Major issues)</h3>
          <p v-if="overview[1] > 500">* Showing statistics for 500 issues</p>
          <BarChart
            :chart-data="{ datasets: [{ label: 'Issues', data: level1 }] }"
          />
        </div>
        <div class="component" v-if="overview[2] > 0">
          <h3>Level 2 (Intermediate issues)</h3>
          <p v-if="overview[2] > 500">* Showing statistics for 500 issues</p>
          <BarChart
            :chart-data="{ datasets: [{ label: 'Issues', data: level2 }] }"
          />
        </div>

        <div class="component" v-if="overview[3] > 0">
          <h3>Level 3 (Minor issues)</h3>
          <p v-if="overview[3] > 500">* Showing statistics for 500 issues</p>
          <BarChart
            :chart-data="{ datasets: [{ label: 'Issues', data: level3 }] }"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
// import router from "../router";
import SearchBar from "../components/SearchBar.vue";
import UserProfile from "../components/UserProfile.vue";
import OsmoseOverview from "../components/OsmoseOverview.vue";
import BarChart from "../components/BarChart.vue";

export default {
  name: "HomePage",
  components: { SearchBar, UserProfile, OsmoseOverview, BarChart },
  data() {
    return {
      username: "",
      selectedUser: {},
      searchResults: [],
      userSelected: false,
      osmProfile: {},
      overview: {},
      level1: {},
      level2: {},
      level3: {},
      top500: {},
    };
  },
  methods: {
    async getSearch(event) {
      this.username = event;
      try {
        let whosthat = await axios.get(
          `https://whosthat.osmz.ru/whosthat.php?action=names&name=${event}`
        );
        this.searchResults = whosthat.data;
      } catch (error) {
        console.log(error);
      }
    },
    async selectUser(user) {
      Object.assign(this.selectedUser, user);
      this.username = null;
      this.searchResults = [];
      this.userSelected = true;
      await this.getOsmProfile();
      await this.getOsmoseData();
    },
    async getOsmProfile() {
      try {
        const { user } = (
          await axios.get(
            `https://api.openstreetmap.org/api/0.6/user/${this.selectedUser.id}.json`
          )
        ).data;
        this.osmProfile = user;
      } catch (error) {
        console.log(error);
      }
    },
    async getOsmoseData() {
      try {
        await this.osmProfile;
        const overview = (
          await axios.get(
            `https://osmose.openstreetmap.fr/en/byuser_count/${this.osmProfile.display_name}`
          )
        ).data;
        const level1 = (
          await axios.get(
            `https://osmose.openstreetmap.fr/api/0.3/user/${this.osmProfile.display_name}?level=1`
          )
        ).data;
        const level2 = (
          await axios.get(
            `https://osmose.openstreetmap.fr/api/0.3/user/${this.osmProfile.display_name}?level=2`
          )
        ).data;
        const level3 = (
          await axios.get(
            `https://osmose.openstreetmap.fr/api/0.3/user/${this.osmProfile.display_name}?level=3`
          )
        ).data;
        // const top500 = (
        //   await axios.get(
        //     `https://osmose.openstreetmap.fr/en/byuser/${this.osmProfile.display_name}.json?`
        //   )
        // ).data;

        this.overview = overview;
        this.level1 = this.createCounts(level1);
        this.level2 = this.createCounts(level2);
        this.level3 = this.createCounts(level3);
      } catch (error) {
        console.log(error);
      }
    },
    createCounts(data) {
      const counts = data.issues
        .map((issue) => issue.menu.en)
        .reduce((accum, i) => {
          accum[i] = accum[i] ? accum[i] + 1 : 1;
          return accum;
        }, {});
      return counts;
    },
  },
};
</script>

<style scoped>
#search-results {
  background-color: white;
  width: 350px;
  border-radius: 5px;
  margin: 1rem auto;
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

.summary {
  display: flex;
  justify-content: center;
  column-gap: 1rem;
}

#levels {
  display: flex;
  flex-direction: column;
  row-gap: 2rem;
  margin-top: 1rem;
  text-align: center;
}

@media screen and (max-width: 799px) {
  #homepage {
    width: 96%;
    margin: auto;
  }
  .summary {
    flex-direction: column;
    row-gap: 1rem;
  }
}
</style>
