<template>
  <tr v-on:click="$emit('highlight-item', id)">
    <td>{{ artist | changeEmpty }}</td>
    <td>{{ album | changeEmpty }}</td>
    <td>{{ track | changeEmpty }}</td>
    <td>{{ scrobbleDate | changeEmpty }}</td>
  </tr>
</template>

<script>
export default {
  props: {
    artist: String,
    album: String,
    track: String,
    scrobbleDate: Date,
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
</style>
