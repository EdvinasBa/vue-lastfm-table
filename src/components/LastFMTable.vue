<template>
  <section>
    <div class="info">
      <span class="info--text">Current page: <strong> {{ currentPage }} </strong> </span>
      <span class="info--text">Number of items: <strong> {{ tableData.length }} </strong></span>
      <span class="info--text">Currently sorting by: <strong> {{ currentlySortedBy }} </strong> </span>
      <label class="info--text">
        Show all entries? [ may be slow with too many entries ]
        <input type="checkbox" v-model="showAllEntries" />
      </label>
    </div>
    <table>
      <thead>
        <tr>
          <th v-on:click="sortBy('Artist')">Artist</th>
          <th v-on:click="sortBy('Album')">Album</th>
          <th v-on:click="sortBy('Track')">Track</th>
          <th v-on:click="sortBy('Date')">Date</th>
        </tr>
      </thead>
      <tbody v-if="showAllEntries">
        <LastFMEntry
          v-for="(item, index) in tableData"
          v-bind:key="index"
          :artist="item.Artist"
          :album="item.Album"
          :track="item.Track"
          :scrobbleDate="item.Date"
          :id="index"
        />
      </tbody>
      <tbody v-else>
        <LastFMEntry
          v-for="(item, index) in paginatedData"
          v-bind:key="index"
          :artist="item.Artist | toString"
          :album="item.Album | toString"
          :track="item.Track | toString"
          :scrobbleDate="item.Date"
          :id="index"
        />
      </tbody>
    </table>
    <div class="pagination" v-if="!showAllEntries">
      <button :disabled="currentPage === 0" v-on:click="prevPage">
        Previous
      </button>
      <input
        v-model.number="currentPage"
        :max="pageCount"
        :min="0"
        type="number"
      />
      <button :disabled="currentPage >= pageCount - 1" v-on:click="nextPage">
        Next
      </button>
    </div>
  </section>
</template>

<script>
import LastFMEntry from "@/components/LastFMEntry.vue";

export default {
  name: "LastFMTable",
  components: {
    LastFMEntry
  },
  data() {
    return {
      currentPage: 0,
      currentClicked: -1,
      size: 15,
      showAllEntries: false,
      currentlySortedBy: "Date"
    };
  },
  props: {
    tableData: {
      type: Array,
      required: true
    }
  },
  methods: {
    nextPage() {
      this.currentPage++;
    },
    prevPage() {
      this.currentPage--;
    },
    sortBy(sortKey) {
      if (this.currentlySortedBy === sortKey) {
        this.tableData.reverse();
        return;
      }

      this.currentlySortedBy = sortKey;

      if (sortKey == "Date") {
        this.tableData.sort((a, b) => {
          return new Date(b.date) - new Date(a.date);
        });
      } else {
        this.tableData.sort((a, b) => {
          let first = a[sortKey].toString().toLowerCase() || "";
          let second = b[sortKey].toString().toLowerCase() || "";

          // eslint-disable-next-line
          // first = first.toLowerCase().match(/[A-Za-z0-9]+/g) || "";

          // eslint-disable-next-line
          // second = second.toLowerCase().match(/[A-Za-z0-9]+/g) || "";

          first = first instanceof Array ? first.join("") : first;
          second = second instanceof Array ? second.join("") : second;

          if (first < second) {
            return -1;
          }

          if (first > second) {
            return 1;
          }

          return 0;
        });
      }
      this.currentPage = 0;
    }
  },
  computed: {
    pageCount() {
      let l = this.tableData.length,
        s = this.size;
      return Math.floor(l / s);
    },
    paginatedData() {
      const start = this.currentPage * this.size,
        end = start + this.size;
      return this.tableData.slice(start, end);
    }
  },
  filters: {
    toString: value => {
      return value.toString();
    }
  }
};
</script>

<style>
.info {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  align-items: flex-start;
}

table {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
}
tr {
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: center;
  min-height: 2.5em;
  text-align: left;
  padding: 0 0.5em;
}
tr > th:nth-child(1),
tr > td:nth-child(1) {
  flex: 0 0 20%;
}
tr > th:nth-child(2),
tr > td:nth-child(2) {
  flex: 0 0 30%;
}

tr > th:nth-child(3),
tr > td:nth-child(3) {
  flex: 0 0 30%;
}

tr > th:nth-child(4),
tr > td:nth-child(4) {
  flex: 0 0 20%;
}

tr:nth-child(even) {
  background: #dcf3e8;
}
tr:hover {
  background: #bcf5d9;
}
th {
  cursor: pointer;
}
button {
  cursor: pointer;
  margin: 1em 2em;
  border: none;
  background: #42b983;
  border-radius: 0.2em;
  font-weight: 600;
  color: white;
  padding: 1em 2em;
}

button:disabled {
  background: #dcf3e8;
}
</style>
