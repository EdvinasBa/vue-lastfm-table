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
</style>
