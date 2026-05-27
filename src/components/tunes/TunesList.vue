<template>
  <ul class="tunes-list">
    <TunesListSong v-for="song in newSongs" :key="song.id" :song="song" />
  </ul>
</template>

<script>
  import TunesListSong from './TunesListSong.vue';

  export default {
    components: {
      TunesListSong,
    },
    props:
    ['new-songs']
  }
</script>

<style>
ul {
  padding-left: 0;
  margin-top: 5em;
  list-style: none;
}
</style>
