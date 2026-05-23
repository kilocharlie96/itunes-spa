<template>
  <form class="ean" @submit.prevent="getFood()" action="#">
    <label class="title" for="ean">iFoods</label>
    <input class="input has-text-centered" v-model="query" type="text" inputmode="numeric" pattern="[0-9]*" autofocus
      name="ean" placeholder="Zadaj čiarový (EAN) kód" />

    <ButtonSubmit @submit.prevent="getFood()" />
  </form>
</template>

<script>
import axios from 'axios'
import ButtonSubmit from '@/components/buttons/ButtonSubmit.vue';


export default {
  components: {
    ButtonSubmit
  },
    data() {
    return {
      query: '',
      food: {}
    }
  },
  methods: {
    getFood() {
      axios
        .get(`https://sk.openfoodfacts.org/api/v2/product/${this.query}`)
        .then((response) => {
          console.log(response.data.product)
          this.food = response.data.product

          this.quantity =
            this.food.quantity_imported ||
            this.food.product_quantity + ' ' + this.food.product_quantity_unit
          this.food.ratingsSum = 0
          this.food.avgRating = 0
          this.food.numOfRatings = 0
        })
        .catch((error) => {
          console.error('Chyba pri načítaní dát:', error)
          //TODO
        })
    }
  },
}
</script>

<style scoped>
  .ean label.title {
    display: block;
  }

  .ean input {
  width: 13rem;
}
</style>