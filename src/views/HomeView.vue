<template>
  <v-progress-circular
    v-if="loading"
    class="loader"
    indeterminate
    size="64"
    color="primary"
  ></v-progress-circular>

  <v-table v-else class="my-16 table mx-auto" fixed-header height="500px">
    <thead class="sticky-header">
      <tr>
        <th class="text-left sticky-first-header">API</th>
        <th class="text-left">Description</th>
        <th class="text-left">Auth</th>
        <th class="text-left">HTTPS</th>
        <th class="text-left">Cors</th>
        <th class="text-left">Link</th>
        <th class="text-left">Category</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in apiData" :key="item.API">
        <td>{{ item.API }}</td>
        <td>{{ item.Description }}</td>
        <td>{{ item.Auth }}</td>
        <td>{{ item.HTTPS }}</td>
        <td>{{ item.Cors }}</td>
        <td>{{ item.Link }}</td>
        <td>{{ item.Category }}</td>
      </tr>
    </tbody>
  </v-table>
</template>

<script>
import axios from "axios";

export default {
  name: "HomeView",
  data() {
    return {
      apiData: [],
      loading: true,
    };
  },
  mounted() {
    axios
      .get("https://api.publicapis.org/entries")
      .then((response) => {
        this.apiData = response.data.entries;
        this.loading = false;
      })
      .catch((error) => {
        console.log(error);
        this.loading = false;
      });
  },
};
</script>
<style scoped>
.table {
  width: 50%;
}
.sticky-first-header {
  position: sticky;
  top: 0;
  background-color: #fff;
  z-index: 1;
}

.sticky-column {
  position: sticky;
  left: 0;
  z-index: 1;
  background-color: #fff;
}

.sticky-header th {
  z-index: 1;
}
.loader {
  margin-left: 50%;
  margin-top: 50px;
}
</style>
