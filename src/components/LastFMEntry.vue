<template>
  <tr v-on:click="$emit('show-entry', id)">
    <td>{{ artist | changeEmpty }}</td>
    <td>{{ album | changeEmpty }}</td>
    <td>{{ track | changeEmpty }}</td>
    <td>{{ scrobbleDate | changeEmpty }}</td>
  </tr>
</template>

<script>
export default {
  props: {
    artist: Number | String,
    album: Number | String,
    track: Number | String,
    scrobbleDate: Number | String,
    id: Number
  },
  filters: {
    changeEmpty: value => {
      return value ? value : "-";
    }
  }
};
</script>
