<template>
  <div class="food">
    <FormSearchBarcode/>

    <div>
      <h6 class="title is-6 mt-3 mb-1">Skús napríklad:</h6>
      <ul>
        <li>3017620422003</li>
        <li>5449000000439</li>
        <li>4056489158448</li>
      </ul>
    </div>

    <div class="card is-clipped mb-1" v-if="food.brands">
      <div class="card-image has-background-white">
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
          <p>Celkové hodnotenie: {{ food.avgRating }}</p>

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
              <button
                class="button is-dark p-0 rating-button"
                @click.prevent="rating--"
                :disabled="rating === 0 || isRated"
              >
                -
              </button>
              <input
                class="input has-text-centered"
                v-model="rating"
                type="number"
                min="0"
                max="5"
                name="rating"
                :disabled="isRated"
              />
              <button
                class="button is-dark p-0 rating-button"
                @click.prevent="rating++"
                :disabled="rating === 5 || isRated"
              >
                +
              </button>
            </div>

            <ButtonSubmit class="m-5 mb-4" @submit.prevent="rate()" :disabled="isRated"/>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import StarIcon from '../components/base/icons/IconStar.vue'
import ButtonSubmit from '@/components/buttons/ButtonSubmit.vue';
import FormSearchBarcode from '@/components/tunes/forms/FormSearchBarcode.vue';

export default {
  components: {
    StarIcon,
    ButtonSubmit,
    FormSearchBarcode
  },
  data() {
    return {
      food: {},
      rating: 0,
      quantity: '',
      isRated: false,
    }
  },
  methods: {
    
    rate() {
      if (this.isRated) return

      this.food.totalRating += this.rating
      this.food.numOfRatings++
      this.food.avgRating = (this.food.totalRating / this.food.numOfRatings).toFixed(0)

      this.isRated = true
    },
  },
}
</script>

<style scoped>
/* input[type='number'] {
  appearance: textfield;
}

input[type='number']::-webkit-outer-spin-button,
input[type='number']::-webkit-inner-spin-button {
  appearance: none;
  margin: 0;
} */

.food {
  text-align: center;
  padding-top: 3em;
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

.rating {
  width: 100%;
  display: grid;
  gap: 0.5em;
}

.rating-container {
  display: flex;
  align-items: center;
  gap: 5%;
  justify-content: center;
}

.rating label {
  margin: auto 0;
}

.rating input {
  width: 3em;
}
</style>
