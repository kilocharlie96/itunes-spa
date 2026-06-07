<template>
  <ul class="tunes-list">
    <TunesListSong v-for="song in newSongs.slice(0, limit)" :key="song.id" :song="song" />
  </ul>
  <ButtonAdd @click="increaseLimit" />
</template>

<script>
import TunesListSong from './TunesListSong.vue'
import ButtonAdd from '@/components/base/buttons/ButtonAdd.vue'

export default {
  components: {
    TunesListSong,
    ButtonAdd,
  },
  props: ['new-songs'],
  data() {
    return {
      limit: 5,
    }
  },
  methods: {
    increaseLimit() {
      this.limit = this.limit + 5
    },
  },
}
</script>

<style scoped>
ul {
  padding-left: 0;
  margin-top: 5em;
  list-style: none;
}
</style>
