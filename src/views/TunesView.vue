<template>
  <div class="tunes">
    <h1>iTunes</h1>

    <form action="#" @submit.prevent="getMusic()">
      <input v-model="query" type="text" autofocus placeholder="Čo ideme hľadať?">
    </form>

    <ul>
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
      console.log(this.query);

      axios.get(`https://itunes.apple.com/search?term=${encodeURIComponent(this.query)}&entity=song&limit=${this.limit}`)
        .then(response => {
          this.songs = []

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

<style>
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
