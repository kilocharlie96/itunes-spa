<template>
  <div class="food">
    <form @submit.prevent="getFood()" action="#">
      <input v-model="query" type="text" inputmode="numeric" pattern="[0-9]*" autofocus placeholder="Zadaj čiarový kód">
    </form>

    <div v-if="food.brands">
      <div>
        <img :src="food.image_thumb_url" alt="food image"/>
      </div>

      <p>
        {{ food.brands }}
      </p>

      <p>{{ food.productRating }}</p>

      <form v-if="food.brands" @submit.prevent="rate()" action="#">
        Vaše hodnotenie:
        <input v-model="rating" type="number" min="0" max="5">
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

  export default {
    data() {
      return {
        query: '',
        rating: 0,
        food: {}
      }
    },
    methods: {
      getFood() {
      axios.get(`https://world.openfoodfacts.org/api/v2/product/${this.query}`)
        .then(response => {
          this.food = response.data.product;
          this.food.productRating = 0;
        })
        .catch(error => {
          console.error("Chyba pri načítaní dát:", error);
        });
      },
      rate() {
        this.food.productRating = this.rating;
      }
    },
  }
</script>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
