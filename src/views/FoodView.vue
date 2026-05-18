<template>
  <div class="food">
    <form @submit.prevent="getFood()" action="#">
      <label class="ean" for="ean">Zadaj čiarový kód:</label>
      <input v-model="query" type="text" inputmode="numeric" pattern="[0-9]*" autofocus name="ean" placeholder="EAN kód">
    </form>

    <div class="food-card" v-if="food.brands">
      <div>
        <img :src="food.image_thumb_url" alt="food image"/>
      </div>

      <p>
        {{ food.brands }}
      </p>

      <p>{{ food.product_name }}</p>

      <p>{{ food.quantity }}</p>

      <p>
        Celkové hodnotenie: {{ food.avgRating }}
      </p>
      <p>
        <small>Počet hodnotení: {{ food.numOfRatings }}</small>
      </p>

      <form v-if="food.brands" @submit.prevent="rate()" action="#">
        <label for="rating">Vaše hodnotenie:</label>
        <input v-model="rating" type="number" min="0" max="5" name="rating">
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

// celkovo / pocet = hodnotenie

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
      axios.get(`https://sk.openfoodfacts.org/api/v2/product/${this.query}`)
        .then(response => {
          console.log(response.data.product);
          this.food = response.data.product;
          this.food.totalRating = 0;
          this.food.avgRating = 0;
          this.food.numOfRatings = 0;
        })
        .catch(error => {
          console.error("Chyba pri načítaní dát:", error);
        });
      },
      rate() {
        this.food.totalRating += this.rating;
        this.food.numOfRatings++;
        this.food.avgRating = (this.food.totalRating / this.food.numOfRatings).toFixed(1);
      },
    },
  }
</script>

<style>
  .food {
    text-align: center;
    padding-top: 3em;
  }

  .food-card {
    margin-top: 3em;
  }

  label.ean {
    display: block;
  }
</style>
