<template>
  <form @submit.prevent="getMusic()">
    <input
      class="input has-text-centered"
      v-model="query"
      type="text"
      placeholder="Názov piesne, interpreta, albumu ..."
      ref="searchInput"
    />
    <ButtonSubmit @submit.prevent="getMusic()" />
  </form>
</template>

<script>
import axios from 'axios'
import ButtonSubmit from '@/components/base/buttons/ButtonSubmit.vue'

export default {
  components: {
    ButtonSubmit,
  },
  data() {
    return {
      query: '',
    }
  },
  methods: {
    getMusic() {
      axios
        .get(`https://itunes.apple.com/search?term=${encodeURIComponent(this.query)}&entity=song`)
        .then((response) => {
          let iTunesSongs = response.data.results
            .filter((song) => song.kind === 'song')
            .map((song) => this.extractData(song))

          this.$emit('add-new-songs', iTunesSongs)
        })
        .catch((error) => {
          console.error('Chyba pri načítaní dát:', error)
        })
    },
    extractData({
      trackId: id,
      artistName: artist,
      previewUrl: audioFile,
      artworkUrl100: cover,
      trackName: name,
      collectionName: album,
    }) {
      return { id, artist, audioFile, cover, name, album }
    },
  },
  mounted() {
    this.$refs.searchInput.focus()
  },
}
</script>

<style scoped>
input {
  width: 17rem;
}
</style>
