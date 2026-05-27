<template>
  <li class="tunes-list-song">
    <div v-if="song.cover">
      <img :src="song.cover" alt="album cover image" />
    </div>

    <p>
      <strong>{{ songify(song) }}</strong>
    </p>

    <figure v-if="song.audioFile">
      <figcaption>{{ song.album }}</figcaption>
      <audio controls :src="song.audioFile"></audio>
    </figure>

  </li>
</template>

<script>
export default {
  props: {
    song: {
      type: Object
    },
  },
  methods: {
    songify(song) {
      return song.artist + ' - ' + song.name;
    },
  },
}
</script>

<style>
li {
  margin-top: 2em;
  padding-top: 2em;
  border-top: 1px solid gray;
}
</style>
