<template>
  <div class="tunes">
    <h1 class="title">iTunes</h1>

    <TunesFormSearch @add-new-song="songs.push($event)" />
    <TunesList :new-songs="songs" />

  </div>
</template>

<script>
import TunesFormSearch from '@/components/tunes/TunesFormSearch.vue';
import TunesList from '@/components/tunes/TunesList.vue';

export default {
  components: {
    TunesFormSearch,
    TunesList,
  },
  data() {
    return {
      songs: []
    }
  },
}
</script>

<style scoped>
.tunes {
  text-align: center;
  padding-top: 2em;
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
