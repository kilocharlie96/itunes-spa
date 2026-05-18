<template>
  <div class="food">
    <form @submit.prevent="getFood()" action="#">
      <label class="ean title" for="ean">iFoods</label>
      <input class="input" v-model="query" type="text" inputmode="numeric" pattern="[0-9]*" autofocus name="ean"
        placeholder="Zadaj čiarový (EAN) kód">
    </form>



    <div>
      <h6 class="title is-6 mt-3 mb-1">Skús napríklad:</h6>
      <ul>
        <li>3017620422003</li>
        <li>5449000000439</li>
        <li>4056489158448</li>
      </ul>
</div>

    <div class="card" v-if="food.brands">
      <div class="card-image">
        <figure class="image">
          <img :src="food.image_url" alt="food image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{ food.product_name }}</p>
            <p class="subtitle is-6">{{ food.brands }}</p>
          </div>
        </div>

        <div class="content">
          <p>
            Celkové hodnotenie: {{ food.avgRating }}
          </p>
          <p>
            <small>Počet hodnotení: {{ food.numOfRatings }}</small>
          </p>

          <form v-if="food.brands" @submit.prevent="rate()" action="#">
            <label for="rating">Vaše hodnotenie:</label>
            <input class="input" v-model="rating" type="number" min="0" max="5" name="rating">
          </form>
        </div>
      </div>
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

<style scoped>
  .food {
    text-align: center;
    padding-top: 3em;
  }

  label.ean {
    display: block;
  }

  .card {
    margin: 3em auto 0 auto;
    max-width: 25rem;
  }

  .image {
    margin: 0 auto;
  }
</style>
