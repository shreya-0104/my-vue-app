<template>
  <v-container>
    <!-- <h3>{{ $route.params.id }}</h3> -->
    <v-row justify="center">
      <v-col v-for="items in cards" :key="items.id">
        <v-card class="mx-auto my-16" max-width="344">
          <div class="remove-button">
            <v-btn append-icon="mdi-close" @click="removeItem(items.id)">
            </v-btn>
          </div>

          <v-img :src="items.image" height="200px" cover></v-img>

          <v-card-title>
            {{ items.title }}
          </v-card-title>

          <v-card-subtitle>
            {{ items.price }}
          </v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
    <v-row v-if="isLoading" justify="center">
      <v-progress-circular indeterminate color="primary"></v-progress-circular>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      cards: [],
      card: null,
      isLoading: true,
    };
  },
  created() {
    const cardId = this.$route.params.id;
    this.card = cardId;
  },
  activated() {
    const cardId = this.$route.params.id;
    const cardIndex = this.cards.findIndex((card) => card.id === cardId);
    if (cardIndex >= 0) {
      this.$refs.cards[cardIndex].highlight();
    }
  },
  mounted() {
    axios
      .get("https://fakestoreapi.com/products/")
      .then((response) => {
        this.cards = response.data;
        this.isLoading = false;
      })
      .catch((error) => {
        console.log(error);
      });
  },

  methods: {
    removeItem(id) {
      const index = this.cards.findIndex((item) => item.id === id);
      if (index !== -1) {
        this.cards.splice(index, 1);
      }
    },
  },
};
</script>
<style scoped>
.remove-button {
  text-align: right;
}
</style>