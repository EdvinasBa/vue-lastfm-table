<template>
  <section class="last-fm-data">
    <div class="info">
      <div class="info__general">
        <strong>General info:</strong>
        <span>
          Current page:
          <strong>{{ currentPage }}</strong>
        </span>
        <span>
          Number of items:
          <strong>{{ tableData.length }}</strong>
        </span>
        <span>
          Currently sorting by:
          <strong>{{ currentlySortedBy }}</strong>
        </span>
        <label>
          Show all entries? [ may be slow with too many entries ]
          <input
            type="checkbox"
            v-model="showAllEntries"
          >
        </label>
      </div>
      <div v-if="currentClicked !== -1" class="info__clicked">
        <strong>Last clicked element:</strong>
        <span>
          Artist:
          <strong>{{ currentClickedElement.Artist }}</strong>
        </span>
        <span>
          Album:
          <strong>{{ currentClickedElement.Album }}</strong>
        </span>
        <span>
          Track:
          <strong>{{ currentClickedElement.Track }}</strong>
        </span>
        <span>
          Date:
          <strong>{{ currentClickedElement.Date }}</strong>
        </span>
      </div>
      <div class="pagination" v-if="!showAllEntries">
        <button :disabled="currentPage === 0" v-on:click="prevPage">Previous</button>
        <input v-model.number="currentPage" :max="pageCount" :min="0" type="number">
        <button :disabled="currentPage >= pageCount - 1" v-on:click="nextPage">Next</button>
      </div>
    </div>
    <div class="data">
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
            v-on:show-entry="changeEntry"
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
            v-on:show-entry="changeEntry"
            :artist="item.Artist"
            :album="item.Album"
            :track="item.Track"
            :scrobbleDate="item.Date"
            :id="index"
          />
        </tbody>
      </table>
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
      this.currentClicked = -1;

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
    },
    changeEntry(id) {
      this.currentClicked = id;
    },
    paginateData() {
      const start = this.currentPage * this.size,
        end = start + this.size;
      return this.tableData.slice(start, end);
    }
  },
  computed: {
    pageCount() {
      let l = this.tableData.length,
        s = this.size;
      return Math.floor(l / s);
    },
    paginatedData() {
      return this.paginateData();
    },
    currentClickedElement() {
      if (this.showAllEntries) {
        console.log(this.paginateData()[this.currentClicked]);
        return this.tableData[this.currentClicked];
      } else {
        console.log(this.paginateData()[this.currentClicked]);
        return this.paginateData()[this.currentClicked];
      }
    }
  }
};
</script>

<style>
.info__general,
.info__clicked {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-bottom: 1px solid #e1e1e1;
  margin: 1em 0;
  padding: 1em 0;
}
.info__general {
  margin-right: 1em;
  padding-right: 1em;
}
.info {
  flex: 0 0 30%;
  position: sticky;
  top: 0;
  max-height: 90vh;
  padding: 1em 0;
  text-align: left;
  line-height: 1.5;
}
.data {
  flex: 0 0 70%;
}
.last-fm-data {
  display: flex;
}
tr {
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: center;
  text-align: left;
  padding: 0 0.5em;
  border-bottom: 0.1rem solid #e1e1e1;
}
tr > th, tr > td {
  border-bottom: none;
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
  background: rgba(155,77,202,0.05);
}
tr:hover {
  cursor: pointer;
  background: rgba(155,77,202,0.2);
}
.pagination {
  display: flex;
  justify-content: space-between;
  margin: 1em auto;
  max-width: 25em;
}
.pagination input {
  width: auto;
}
</style>
