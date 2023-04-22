  <template>
  <v-card>
    <v-layout>
      <v-app-bar :dark="darkTheme" color="indigo-darken-1" prominent>
        <v-btn @click="toggleTheme"
          >{{ darkTheme ? "Light" : "Dark" }} Theme</v-btn
        >
        <div>
          <router-link to="/" class="nav">Home </router-link>
          <router-link to="/cards" class="nav">Cards</router-link>
        </div>

        <v-spacer></v-spacer>

        <v-autocomplete
          append-inner-icon="mdi-magnify"
          v-model="searchQuery"
          :items="searchResults"
          item-text="title"
          item-value="id"
          placeholder="Search for cards"
          @input="handleSearch"
          @change="$router.push('/cards/' + selectedCardId)"
        ></v-autocomplete>
      </v-app-bar>
      <v-main style="height: 10px"> </v-main>
    </v-layout>
  </v-card>
</template>
  <script>
import { useTheme } from "vuetify";
import { ref } from "vue";
import axios from "axios";
export default {
  name: "NavBar",
  data() {
    return {
      searchQuery: "",
      searchResults: [],
      cards: [],
      themes: [
        {
          name: "light",
          value: false,
        },
        {
          name: "dark",
          value: true,
        },
      ],
    };
  },
  computed: {
    selectedCardId() {
      return this.searchResults[0] ? this.searchResults[0].id : "";
    },
  },
  watch: {
    selectedItemId(id) {
      if (id) {
        this.$router.push(`/cards/${id}`);
      }
    },
  },
  methods: {
    handleSearch() {
      this.searchResults = this.cards.filter(
        (card) =>
          card.title.includes(this.searchQuery) ||
          card.description.includes(this.searchQuery)
      );
    },
  },
  setup() {
    const theme = useTheme();
    const darkTheme = ref(false);

    const toggleTheme = () => {
      darkTheme.value = !darkTheme.value;
      theme.global.name.value = theme.global.current.value.dark
        ? "light"
        : "dark";
    };
    return {
      theme,
      toggleTheme,
      darkTheme,
    };
  },
  mounted() {
    axios
      .get("https://fakestoreapi.com/products/")
      .then((response) => {
        this.cards = response.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>
  <style scoped>
.nav {
  margin-left: 30px;
  text-decoration: none;
  color: white;
}
.nav:hover {
  color: black;
}
</style>