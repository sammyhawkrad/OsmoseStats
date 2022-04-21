<template>
  <div>
    <div class="osm-profile" v-if="osmProfile !== {}">
      <div id="profile-picture">
        <img
          v-if="osmProfile.img"
          :src="osmProfile.img.href"
          alt="Profile picture"
        />
        <img v-else src="../assets/user.png" alt="Profile picture" />
      </div>
      <div>
        <p id="display-name">{{ osmProfile.display_name }}</p>
        <p v-if="osmProfile.account_created">
          Account created:
          <span class="info-span">{{
            slicedate(osmProfile.account_created)
          }}</span>
        </p>
        <p v-if="osmProfile.changesets">
          Changesets:
          <span class="info-span">{{ osmProfile.changesets.count }}</span>
        </p>
        <p v-if="osmProfile.blocks">
          Blocks:
          <span class="info-span">{{ osmProfile.blocks.received.count }}</span>
          <span v-if="osmProfile.blocks.received.active > 0"
            >(Active: {{ osmProfile.blocks.received.active }} )</span
          >
        </p>
        <div class="links">
          <a
            :href="`https://www.hdyc.neis-one.org/?${osmProfile.display_name}`"
            target="_blank"
            id="hdyc"
            >HDYC</a
          >
          <a
            :href="`https://www.osm.org/user/${osmProfile.display_name}`"
            target="_blank"
            ><img src="../assets/osm.svg" alt=""
          /></a>
          <a
            :href="`https://www.missingmaps.org/users/#/${osmProfile.display_name}`"
            target="_blank"
            ><img src="../assets/missingmaps.svg" alt=""
          /></a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "UserProfile",
  props: { selectedUser: Object, osmProfile: Object },
  methods: {
    slicedate(date) {
      return date.slice(0, 10);
    },
  },
};
</script>

<style scoped>
.osm-profile {
  background-color: white;
  border-radius: 10px;
  width: 25rem;
  margin: auto;
  padding: 1rem;
  box-shadow: 0 0 25px rgba(0, 0, 0, 0.04);
  display: flex;
  justify-content: space-around;
  min-height: 170px;
}

#profile-picture {
  align-self: center;
}

#profile-picture > img {
  height: 90px;
  border-radius: 50%;
  border: 2px solid #ffd449;
}

#display-name {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.info-span {
  color: darkslategrey;
}

.links {
  display: flex;
  justify-content: flex-start;
  margin-top: 1rem;
  column-gap: 1rem;
}

.links > a > img {
  height: 25px;
}

#hdyc {
  border-radius: 5px;
  background-color: rgb(56, 55, 55);
  color: white;
  padding: 2px 10px;
  line-height: 1rem;
  text-decoration: none;
}
</style>
