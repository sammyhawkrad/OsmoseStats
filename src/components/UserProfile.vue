<template>
  <div>
    <div>
      <img
        v-if="osmProfile.img"
        :src="osmProfile.img.href"
        alt="Profile picture"
      />
      <p>Display name: {{ osmProfile.display_name }}</p>
      <p>Account created: {{ osmProfile.account_created }}</p>
      <p v-if="osmProfile.changesets">
        Changesets: {{ osmProfile.changesets.count }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "UserProfile",
  props: { selectedUser: Object, osmProfile: Object },
};
</script>

<style scoped></style>
