<template>
  <div class="tunes">
    <h1>iTunes</h1>

    <form action="#" @submit.prevent="getMusic()">
      <input v-model="query" type="text">
    </form>

    <ul>
      <li v-for="song in songs" :key="song.id">
        {{ songify(song) }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      query: '',
      songs: [
        { id: 1, artist: 'Great Artist', name: 'Great Song' },
        { id: 2, artist: 'Samčo, brat dážďoviek', name: 'Soros mi daroval dlažobnú kocku' },
        { id: 3, artist: 'IMT Sad', name: 'Preafektovaná' }
      ]
    }
  },
  methods: {
    songify(song) {
      return song.artist + ' - ' + song.name;
    },
    getMusic() {
      console.log(this.query)
    }
  },
}
</script>

<style>
ul {
  padding-left: 0;
  margin-top: 2em;
}

@media (min-width: 1024px) {
  .tunes {
    margin: auto 0;
  }
}
</style>
