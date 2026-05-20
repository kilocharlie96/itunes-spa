<template>
  <div class="food">
    <form class="ean" @submit.prevent="getFood()" action="#">
      <label class="title" for="ean">iFoods</label>
      <input class="input has-text-centered" v-model="query" type="text" inputmode="numeric" pattern="[0-9]*" autofocus
        name="ean" placeholder="Zadaj čiarový (EAN) kód">
    </form>

    <div>
      <h6 class="title is-6 mt-3 mb-1">Skús napríklad:</h6>
      <ul>
        <li>3017620422003</li>
        <li>5449000000439</li>
        <li>4056489158448</li>
      </ul>
    </div>

    <div class="card mb-6" v-if="food.brands">
      <div class="card-image has-background-white is-clipped">
        <figure class="image">
          <img :src="food.image_url" class="is-radiusless" alt="food image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{ food.product_name }}</p>
            <p class="subtitle is-6">{{ food.brands }}</p>
            <p class="subtitle is-6 p-1">{{ quantity }}</p>
          </div>
        </div>

        <div class="content">
          <p>
            Celkové hodnotenie: {{ food.avgRating }}
          </p>

          <div>
            <StarIcon :class="{ filled: food.avgRating >= 1 }" />
            <StarIcon :class="{ filled: food.avgRating >= 2 }" />
            <StarIcon :class="{ filled: food.avgRating >= 3 }" />
            <StarIcon :class="{ filled: food.avgRating >= 4 }" />
            <StarIcon :class="{ filled: food.avgRating >= 5 }" />
          </div>

          <p>
            <small>Počet hodnotení: {{ food.numOfRatings }}</small>
          </p>

          <form class="rating" v-if="food.brands" @submit.prevent="rate()" action="#">
            <label for="rating">Ohodnotiť:</label>

            <div class="rating-container">
              <button class="button is-danger p-0 rating-button" @click.prevent="rating--"
                :disabled="rating === 0 || isRated">-</button>
              <input class="input has-text-centered" v-model="rating" type="number" min="0" max="5" name="rating"
                :disabled="isRated">
              <button class="button is-success p-0 rating-button" @click.prevent="rating++"
                :disabled="rating === 5 || isRated">+</button>
            </div>



            <button class="button is-success" @submit.prevent="rate()" :disabled="isRated">
              <svg viewBox="0 0 16 16" height="25" width="25" fill="none" xmlns="http://www.w3.org/2000/svg">
                <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
                <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
                <g id="SVGRepo_iconCarrier">
                  <path d="M8 6L8 2L10 2L16 8L10 14L8 14L8 10L-1.74845e-07 10L-3.01991e-07 6L8 6Z" fill="#fff"></path>
                </g>
              </svg>
            </button>

          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import StarIcon from '../components/icons/IconStar.vue'

export default {
  components: {
    StarIcon
  },
  data() {
    return {
      query: '',
      rating: 0,
      food: {},
      quantity: '',
      isRated: false
    }
  },
  methods: {
    getFood() {
      axios.get(`https://sk.openfoodfacts.org/api/v2/product/${this.query}`)
        .then(response => {
          console.log(response.data.product);
          this.food = response.data.product;
          console.log(this.food.quantity_imported);
          console.log(this.food.product_quantity);
          console.log(this.food.product_quantity_unit);
          this.quantity = this.food.quantity_imported || (this.food.product_quantity + ' ' + this.food.product_quantity_unit);
          this.food.totalRating = 0;
          this.food.avgRating = 0;
          this.food.numOfRatings = 0;
        })
        .catch(error => {
          console.error("Chyba pri načítaní dát:", error);
        });
    },
    rate() {
      if (this.isRated) return;

      this.food.totalRating += this.rating;
      this.food.numOfRatings++;
      this.food.avgRating = (this.food.totalRating / this.food.numOfRatings).toFixed(0);

      this.isRated = true;
    },
  },
}
</script>

<style scoped>
input[type=number] {
  appearance: textfield;
}

input[type=number]::-webkit-outer-spin-button,
input[type=number]::-webkit-inner-spin-button {
  appearance: none;
  margin: 0;
}

.food {
  text-align: center;
  padding-top: 3em;
}

.ean label.title {
  display: block;
}

.card {
  margin: 3em auto 0 auto;
  max-width: 25rem;
}

img {
  width: auto;
  max-height: 400px;
  margin: 0 auto;
}

.filled {
  fill: #f5c542;
}

button.rating-button {
  height: 1.5em;
  width: 1.5em;
}

.rating-container, .rating {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  gap: 2%;
}

.rating label {
  margin: auto 0;
}

.ean input {
  width: 12em;
}

.rating input {
  width: 3em;
}
</style>
