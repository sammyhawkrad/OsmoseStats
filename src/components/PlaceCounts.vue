<template>
  <div>
    <div>
      <table>
        <thead>
          <th>Country / Region</th>
          <th>Number of Issues</th>
        </thead>
        <tbody>
          <tr v-for="place in places" :key="place[0]">
            <td>{{ capitalise(place[0]) }}</td>
            <td>{{ place[1] }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "PlacesCounts",
  props: { places: Array },
  methods: {
    capitalise(str) {
      return str.replace(/^\w/, (c) => c.toUpperCase());
    },
  },
};
</script>

<style scoped>
table {
  border-spacing: 1;
  border-collapse: collapse;
  background: white;
  border-radius: 6px;
  overflow: hidden;
  max-width: 800px;
  width: 100%;
  margin: 0 auto;
  position: relative;
}

th {
  background-color: #ffd449;
  color: black;
}

th,
td {
  padding: 5px;
  text-align: left;
}

th,
td {
  border-bottom: 1px solid #ddd;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}
</style>
