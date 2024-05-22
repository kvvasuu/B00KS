<template>
  <div class="header">
    <div class="header-title">B00KS</div>
    <div v-if="!loading" class="input-container">
      <div v-if="!booksFound" class="header-text">
        <div class="header-description">
          B00KS is your window to the world of the latest literary bestsellers,
          straight from The New York Times. Our app, integrated with the NYT
          API, updates daily with the top books across various categories. Dive
          into the world of literature, discover new titles, and stay up-to-date
          with what everyone is reading.
        </div>

        <div class="header-caption">Select list</div>
      </div>

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
      <select v-if="booksFound" class="select-sort" name="select-sort" @change="sortBooks($event.target.value)">
        <option value="rank_asc">Rank (ascending) &#11165</option>
        <option value="rank_desc">Rank (descending) &#11167</option>
        <option value="weeks_asc">Weeks on list (ascending) &#11165</option>
        <option value="weeks_desc">Weeks on list (descending) &#11167</option>
      </select>
    </div>
    <div v-else class="spinner">
      <div class="lds-dual-ring"></div>
      <transition name="bounce">
        <span class="loading" v-if="requestsLimit"
          >Too many requests.<br />
          Please wait</span
        ></transition
      >
    </div>
  </div>
  <div v-if="!loading" class="items-container">
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
      booksFound: false,
      requestsLimit: false,
    };
  },
  methods: {
    async getListNames() {
      try {
        this.loading = true;
        const response = await fetch(
          `${this.listNamesURL}?api-key=${this.apiKey}`
        );
        const resp = await response.json();
        this.lists = resp.results;
        if (resp.fault) {
          this.requestsLimit = true;
          setTimeout(() => {
            this.loading = false;
            this.requestsLimit = false;
            this.requestsLimitCounter = 30;
            this.getListNames();
          }, 30000);
        } else {
          this.loading = false;
          this.requestsLimitCounter = 30;
        }
      } catch (error) {
        console.error(error);
      }
    },
    async getBooks() {
      this.loading = true;
      try {
        const response = await fetch(this.booksURL);
        const resp = await response.json();
        this.books = resp.results.books;
        this.loading = false;
        this.booksFound = true;
        this.requestsLimitCounter = 30;
      } catch (error) {
        this.requestsLimit = true;
        setTimeout(() => {
          this.loading = false;
          this.requestsLimit = false;
          this.requestsLimitCounter = 30;
        }, 30000);
        console.error(error);
      }
    },
    sortBooks(type){ //not working - try computed(vue does not see sort as change of array)
      type = [...type.split("_")]
      if(type[1] = "asc"){
        this.books.sort((a, b) => a.rank - b.rank)
      }
      else{
        this.books.sort((a, b) => b.rank - a.rank)
      }
      console.log(type)
    }
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
  flex-direction: column;
}

.header-text {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.header-title {
  font-size: 3rem;
  margin: 1rem;
  font-weight: bold;
  filter: drop-shadow(0.1rem 0.1rem 0.2rem rgba(0, 0, 0, 0.3));
}

.header-description {
  margin: 0 1rem 1rem 1rem;
  max-width: 40rem;
  text-align: center;
  color: #181818be;
}

.header-caption {
  font-weight: bold;
  margin: 0.6rem;
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
  border-radius: 0.25rem;
  box-shadow: 0 0 1rem 0 rgba(0, 0, 0, 0.2);
  cursor: pointer;
}

.select-sort {
  appearance: none;
  border: 0;
  outline: 0;
  font: inherit;
  font-size: 0.8rem;
  padding: 0.5rem;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 0.25rem;
  box-shadow: 0 0 1rem 0 rgba(0, 0, 0, 0.2);
  cursor: pointer;
  margin: 1rem 0 0 0;
}

.items-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  margin: 2rem 0 2rem 0;
}

/* Loading spinner */

.spinner {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.spinner span {
  font-size: 0.8rem;
  color: #242424be;
  text-align: center;
  margin-bottom: 0.5rem;
}

.lds-dual-ring,
.lds-dual-ring:after {
  box-sizing: border-box;
}
.lds-dual-ring {
  display: inline-block;
  width: 5rem;
  height: 5rem;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 4rem;
  height: 4rem;
  margin: 0.5rem;
  border-radius: 50%;
  border: 0.4rem solid currentColor;
  border-color: currentColor transparent currentColor transparent;
  animation: lds-dual-ring 2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}

.loading:after {
  overflow: hidden;
  display: inline-block;
  vertical-align: bottom;
  animation: ellipsis steps(4, end) 2000ms infinite;
  content: "\2026";
  width: 0px;
}

@keyframes ellipsis {
  to {
    width: 1rem;
  }
}
</style>
