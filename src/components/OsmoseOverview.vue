<template>
  <div class="overview component">
    <div>
      <h2>Issues Overview</h2>
      <p>
        You have
        <span>{{
          overview[1] + overview[2] + overview[3] > 0
            ? overview[1] + overview[2] + overview[3]
            : "no"
        }}</span>
        OpenStreetMap issues flagged by Osmose!
      </p>
    </div>

    <div class="card-group">
      <div id="level1" class="card" title="Major issues">
        <p class="count">{{ overview[1] }}</p>
        <p class="label">Level 1</p>
      </div>
      <div id="level2" class="card" title="Intermediate issues">
        <p class="count">{{ overview[2] }}</p>
        <p class="label">Level 2</p>
      </div>
      <div id="level3" class="card" title="Minor issues">
        <p class="count">{{ overview[3] }}</p>
        <p class="label">Level 3</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "OsmoseOverview",
  props: { overview: Object },
};
</script>

<style scoped>
.overview {
  min-height: 170px;
  text-align: center;
}
.card-group {
  display: flex;
  justify-content: center;
  column-gap: 0.5rem;
  margin-top: 15px;
}

.card {
  border-radius: 5px;
  color: white;
  padding: 0.2rem 1.5rem;
}

.count {
  font-size: 1.5rem;
  font-weight: bold;
}

#level1 {
  background-color: #d00000;
}

#level2 {
  background-color: #e85d04;
}

#level3 {
  background-color: #ffba08;
}
</style>
