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
        v-for="user in searchResults.slice(0, 4)"
        :key="user.id"
        @click="selectUser(user)"
      >
        {{ user.names[user.names.length - 1] }}
      </li>
    </div>
    <div v-if="userSelected">
      <div class="summary">
        <UserProfile :selectedUser="selectedUser" :osmProfile="osmProfile" />
        <OsmoseOverview :overview="overview" :loading="loading" />
      </div>
      <div id="levels">
        <div class="component" v-if="overview[1] > 0">
          <h3>
            Level 1 (Major issues)
            <span title="View issues">
              <a
                :href="`https://osmose.openstreetmap.fr/en/byuser/${osmProfile.display_name}?level=1`"
                target="_blank"
                rel="noopener noreferrer"
              >
                <img src="../assets/resize.png" />
              </a>
            </span>
          </h3>
          <p v-if="overview[1] > 500">* Showing statistics for 500 issues</p>
          <BarChart
            :chart-data="{
              labels: Object.keys(level1),
              datasets: [
                { axis: 'y', label: 'Issues', data: Object.values(level1) },
              ],
            }"
            :colour="level1colour"
          />
        </div>

        <div class="component" v-if="overview[2] > 0">
          <h3>
            Level 2 (Intermediate issues)
            <span title="View issues">
              <a
                :href="`https://osmose.openstreetmap.fr/en/byuser/${osmProfile.display_name}?level=2`"
                target="_blank"
                rel="noopener noreferrer"
              >
                <img src="../assets/resize.png" alt="" />
              </a>
            </span>
          </h3>

          <p v-if="overview[2] > 500">* Showing statistics for 500 issues</p>
          <BarChart
            :chart-data="{
              labels: Object.keys(level2),
              datasets: [
                { axis: 'y', label: 'Issues', data: Object.values(level2) },
              ],
            }"
            :colour="level2colour"
          />
        </div>

        <div class="component" v-if="overview[3] > 0">
          <h3>
            Level 3 (Minor issues)
            <span title="View issues">
              <a
                :href="`https://osmose.openstreetmap.fr/en/byuser/${osmProfile.display_name}?level=3`"
                target="_blank"
                rel="noopener noreferrer"
              >
                <img src="../assets/resize.png" alt="" />
              </a>
            </span>
          </h3>
          <p v-if="overview[3] > 500">* Showing statistics for 500 issues</p>
          <BarChart
            :chart-data="{
              labels: Object.keys(level3),
              datasets: [
                { axis: 'y', label: 'Issues', data: Object.values(level3) },
              ],
            }"
            :colour="level3colour"
          />
        </div>

        <div class="component" v-if="places.length > 0">
          <PlaceCounts :places="places" />
          <p v-if="overview[1] + overview[2] + overview[3] > 500">
            * Showing statistic for only 500 issues
          </p>
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
import PlaceCounts from "../components/PlaceCounts.vue";

export default {
  name: "HomePage",
  components: { SearchBar, UserProfile, OsmoseOverview, BarChart, PlaceCounts },
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
      places: [],
      loading: true,
      level1colour: "#d00000",
      level2colour: "#e85d04",
      level3colour: "#ffba08",
    };
  },
  methods: {
    reset() {
      this.searchResults = [];
      this.osmProfile = {};
      this.overview = {};
      this.level1 = {};
      this.level2 = {};
      this.level3 = {};
      this.places = [];
      this.loading = true;
    },
    async getSearch(event) {
      this.username = event;
      try {
        let whosthat = await axios.get(
          `https://whosthat.osmz.ru/whosthat.php?action=names&q=${event}`
        );
        this.searchResults = whosthat.data;
      } catch (error) {
        console.log(error);
      }
    },
    async selectUser(user) {
      Object.assign(this.selectedUser, user);
      this.username = "";
      this.searchResults = [];
      this.userSelected = true;
      await this.getOsmProfile();
      await this.getOsmoseData();
      this.loading = false;
    },
    async getOsmProfile() {
      try {
        this.reset();
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
        const OSMOSE_API = "https://osmose.openstreetmap.fr/api/0.3/user";
        await this.osmProfile;

        const overview = (
          await axios.get(
            `https://osmose.openstreetmap.fr/api/0.3/user_count/${this.osmProfile.display_name}`
          )
        ).data;

        const level1 = (
          await axios.get(
            `${OSMOSE_API}/${this.osmProfile.display_name}?level=1`
          )
        ).data;

        const level2 = (
          await axios.get(
            `${OSMOSE_API}/${this.osmProfile.display_name}?level=2`
          )
        ).data;

        const level3 = (
          await axios.get(
            `${OSMOSE_API}/${this.osmProfile.display_name}?level=3`
          )
        ).data;

        const places = (
          await axios.get(`${OSMOSE_API}/${this.osmProfile.display_name}`)
        ).data;

        this.overview = overview;
        this.level1 = this.createCounts(level1);
        this.level2 = this.createCounts(level2);
        this.level3 = this.createCounts(level3);
        this.places = this.countPlaces(places);
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
    countPlaces(data) {
      const counts = data.issues
        .map((issue) => issue.country)
        .reduce((accum, i) => {
          accum[i] = accum[i] ? accum[i] + 1 : 1;
          return accum;
        }, {});
      return Object.entries(counts).sort((a, b) => b[1] - a[1]);
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
