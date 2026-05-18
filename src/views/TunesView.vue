<template>
  <div class="tunes">
    <h1 class="title">iTunes</h1>

    <form action="#" @submit.prevent="getMusic()">
      <input class="input" v-model="query" type="text" autofocus placeholder="Zadaj názov piesne, alebo interpreta">
    </form>

    <ul v-if="isPresent">
      <li v-for="song in songs" :key="song.id">
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
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      query: '',
      limit: 5,
      isPresent: false,
      songs: [{}]
    }
  },
  methods: {
    songify(song) {
      return song.artist + ' - ' + song.name;
    },
    getMusic() {
      console.log(this.query);

      axios.get(`https://itunes.apple.com/search?term=${encodeURIComponent(this.query)}&entity=song&limit=${this.limit}`)
        .then(response => {
          this.songs = []
          this.isPresent = true;

          response.data.results.forEach(song => {
            this.songs.push(this.extractData(song));
          });
        })
        .catch(error => {
          console.error("Chyba pri načítaní dát:", error);
        });
    },
    extractData({
      trackId: id,
      artistName: artist,
      previewUrl: audioFile,
      artworkUrl100: cover,
      trackName: name,
      collectionName: album
    }) {
      return { id, artist, audioFile, cover, name, album };
    }
  },
}
</script>

<style scoped>
  .tunes {
    text-align: center;
    padding-top: 2em;
  }

  ul {
    padding-left: 0;
    margin-top: 5em;
    list-style: none;
  }

  li {
    margin-top: 2em;
    padding-top: 2em;
    border-top: 1px solid gray;
  }

  strong {
    font-weight: bold;
  }

  @media (min-width: 1024px) {
    .tunes {
      margin: auto 0;
    }
  }
</style>
