<template>
  <div class="home">
    <div v-if="tableData.length > 0">
      <LastFMTable :tableData="tableData" />
    </div>
    <h3 v-else>Fetching data....</h3>
  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src
import LastFMTable from "@/components/LastFMTable.vue";

export default {
  name: "home",
  components: {
    LastFMTable
  },
  data() {
    return {
      tableData: []
    };
  },
  created() {
    axios
      .get(
        "https://gist.githubusercontent.com/EdvinasBa/cad49d4ad40a0a87f4566ebcca2f22dd/raw/9cdd3eb4bb340786fff476af45010454c45bb94f/scrobbles.json"
      )
      .then(res => (this.tableData = res.data))
      // eslint-disable-next-line
      .catch(err => console.error(err));
  }
};
</script>
