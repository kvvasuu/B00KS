<template>
  <div class="header">
    <div class="input-container">
      <h2>Choose list</h2>
      <select
        v-model="selectedList"
        @change="getBooks"
        class="select-category"
        name="categories"
      >
        <option v-for="list in lists" :value="list.list_name_encoded">
          {{ list.display_name }}
        </option>
      </select>
    </div>
  </div>
  <div class="items-container">
    <item-card v-for="book in books" :book="book"></item-card>
  </div>
</template>

<script>
import ItemCard from "./components/ItemCard.vue";
export default {
  components: {
    ItemCard,
  },
  data() {
    return {
      lists: {},
      selectedList: "",
      books: {},
      listNamesURL: "https://api.nytimes.com/svc/books/v3/lists/names.json",
      apiKey: "hvuGvTjqrVzQ324M7QjKRnXzThqRlAAg",
      loading: false,
    };
  },
  methods: {
    async getListNames() {
      try {
        const response = await fetch(
          `${this.listNamesURL}?api-key=${this.apiKey}`
        );
        const resp = await response.json();
        this.lists = resp.results;
        console.log(resp.results);
      } catch (error) {
        console.error(error);
      }
    },
    async getBooks() {
      this.loading = true;
      try {
        console.log(this.selectedList);
        const response = await fetch(this.booksURL);
        const resp = await response.json();
        this.books = resp.results.books;
        this.loading = false;
        console.log(resp.results.books);
      } catch (error) {
        console.error(error);
      }
    },
  },
  mounted() {
    this.getListNames();
  },
  computed: {
    booksURL() {
      return `https://api.nytimes.com/svc/books/v3/lists/current/${this.selectedList}.json?api-key=${this.apiKey}`;
    },
  },
};
</script>

<style scoped>
.header {
  height: 10rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.input-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.select-category {
  appearance: none;
  border: 0;
  outline: 0;
  font: inherit;
  width: 20rem;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 0.25em;
  box-shadow: 0 0 1em 0 rgba(0, 0, 0, 0.2);
  cursor: pointer;
}

.items-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  margin: 2rem 0 2rem 0;
}
</style>
