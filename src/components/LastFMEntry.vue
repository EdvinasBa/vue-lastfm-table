<template>
  <tr
    v-on:click="$emit('show-entry', id)"
    v-bind:class="{ 'is-clicked': isClicked }"
  >
    <td>{{ artist | changeEmpty }}</td>
    <td>{{ album | changeEmpty }}</td>
    <td>{{ track | changeEmpty }}</td>
    <td>{{ scrobbleDate | changeEmpty }}</td>
  </tr>
</template>

<script>
export default {
  props: {
    artist: [Number, String],
    album: [Number, String],
    track: [Number, String],
    scrobbleDate: [Number, String],
    isClicked: Boolean,
    id: Number
  },
  filters: {
    changeEmpty: value => {
      return value ? value : "-";
    }
  }
};
</script>

<style>
.is-clicked {
  background: rgba(155, 77, 202, 0.3);
  font-weight: 600;
}
@media screen and (max-width: 768px) {
  thead tr th {
    text-align: left;
    width: 100%;
    padding-left: 0;
  }
  tr {
    display: flex;
    flex-direction: column;
  }
  td {
    width: 100%;
    display: flex;
    justify-content: space-between;
  }
  td:first-child {
    padding-left: 1.5rem;
  }
  td:nth-child(1)::before {
    content: "Artist:";
  }
  td:nth-child(2)::before {
    content: "Album:";
  }
  td:nth-child(3)::before {
    content: "Track:";
  }
  td:nth-child(4)::before {
    content: "Date:";
  }
}
</style>
