<template>
  <div>
    <div>
      <table>
        <thead>
          <th>OSM User</th>
          <th>Number of Objects</th>
        </thead>
        <tbody>
          <tr v-for="partner in pic" :key="partner[0]">
            <td>
              <a
                :href="`https://hdyc.neis-one.org/?${partner[0]}`"
                target="_blank"
                rel="noopener noreferrer"
              >
                {{ partner[0] }}</a
              >
            </td>
            <td>{{ partner[1] }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "PartnerInCrime",
  props: { pic: Array },
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
