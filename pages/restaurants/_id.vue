<template>
  <div>
    <!-- Link to go back to the previous page -->
    <a class="uk-button uk-button-primary uk-margin" @click="$router.go(-1)">
      <span uk-icon="arrow-left"></span> go back
    </a>

    <client-only>
      <div uk-grid>
        <!-- Left card displaying dishes -->
        <div class="uk-width-1-3@m">
          <div v-for="dish in restaurant.dishes" class="uk-margin">
            <div class="uk-card uk-card-default">
              <div class="uk-card-media-top">
                <img :src="'http://localhost:1337' + dish.image[0].url" alt />
              </div>
              <div class="uk-card-body">
                <h3 class="uk-card-title">
                  {{ dish.name }}
                  <span class="uk-badge">{{ dish.price }}€</span>
                </h3>
                <p>{{ dish.description }}</p>
              </div>
              <div class="uk-card-footer">
                <!-- Doing nothing for the moment :) -->
                <button class="uk-button uk-button-primary" @click="addToCart(dish)">Add to cart</button>
              </div>
            </div>
          </div>
        </div>

        <!-- Right card that will display your cart -->
        <div class="uk-width-expand@m">
          <Cart />
        </div>
      </div>
    </client-only>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'
import Cart from '~/components/Cart.vue'
import restaurantQuery from "~/apollo/queries/restaurant/restaurant";

export default {
  data() {
    return {
      restaurant: Object,
    };
  },
  apollo: {
    restaurant: {
      prefetch: true,
      query: restaurantQuery,
      variables() {
        return { id: this.$route.params.id };
      },
    },
  },
  components: {
    Cart,
  },
  methods: {
    ...mapMutations({
      addToCart: "cart/add",
      removeFromCart: "cart/remove",
    }),
  },
};
</script>