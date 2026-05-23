<script setup>
import WelcomeItem from './WelcomeItem.vue'
import BurgerIcon from './icons/IconBurger.vue'
import MusicIcon from './icons/IconMusic.vue'

</script>

<template>
  <WelcomeItem>
    <template #icon>
      <MusicIcon />
    </template>
    <template #heading>Hudba</template>
    Nájdi svoju obľúbenú pesničku vďaka iTunes API.
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <BurgerIcon />
    </template>
    <template #heading>Jedlo</template>
    Nájdi svoju obľúbenú potravinu a zanechaj jej hodnotenie pre ostatných.
  </WelcomeItem>
</template>

<style scoped>
  svg {
    width: 80%;
  }
</style>