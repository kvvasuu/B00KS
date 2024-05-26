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
      <div v-if="booksFound" class="select" tabindex="1">
        <input
          class="selectopt"
          value="rank_asc"
          name="sort"
          type="radio"
          id="opt1"
          @click="sort($event.target.value)"
          checked
        />
        <label for="opt1" class="option"
          >Rank &nbsp
          <i class="fa-solid fa-sort-up" style="margin-top: 0.5rem"></i
        ></label>
        <input
          class="selectopt"
          value="rank_desc"
          name="sort"
          type="radio"
          id="opt2"
          @click="sort($event.target.value)"
        />
        <label for="opt2" class="option"
          >Rank &nbsp
          <i class="fa-solid fa-sort-down" style="margin-top: -0.3rem"></i
        ></label>
        <input
          class="selectopt"
          value="weeks_asc"
          name="sort"
          type="radio"
          id="opt3"
          @click="sort($event.target.value)"
        />
        <label for="opt3" class="option"
          >Weeks on list &nbsp
          <i class="fa-solid fa-sort-up" style="margin-top: 0.5rem"></i
        ></label>
        <input
          class="selectopt"
          value="weeks_desc"
          name="sort"
          type="radio"
          id="opt4"
          @click="sort($event.target.value)"
        />
        <label for="opt4" class="option"
          >Weeks on list &nbsp
          <i class="fa-solid fa-sort-down" style="margin-top: -0.3rem"></i
        ></label>
      </div>
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
    <item-card v-for="book in sortedBooks" :book="book"></item-card>
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
      books: [],
      listNamesURL: "https://api.nytimes.com/svc/books/v3/lists/names.json",
      apiKey: "hvuGvTjqrVzQ324M7QjKRnXzThqRlAAg",
      loading: false,
      booksFound: false,
      requestsLimit: false,
      sortBy: "rank_asc",
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
    sort(type) {
      this.sortBy = type;
    },
  },
  mounted() {
    this.getListNames();
  },
  computed: {
    booksURL() {
      return `https://api.nytimes.com/svc/books/v3/lists/current/${this.selectedList}.json?api-key=${this.apiKey}`;
    },
    sortedBooks() {
      if (this.sortBy.split("_")[0] == "weeks") {
        if (this.sortBy.split("_")[1] == "asc") {
          return [...this.books].sort((a, b) =>
            a.weeks_on_list > b.weeks_on_list
              ? 1
              : b.weeks_on_list > a.weeks_on_list
              ? -1
              : 0
          );
        } else
          return [...this.books]
            .sort((a, b) =>
              a.weeks_on_list > b.weeks_on_list
                ? 1
                : b.weeks_on_list > a.weeks_on_list
                ? -1
                : 0
            )
            .reverse();
      } else if (this.sortBy.split("_")[1] == "desc") {
        return [...this.books].reverse();
      }
      return this.books;
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
  -webkit-appearance: none;
  border: 0;
  outline: 0;
  font: inherit;
  width: 20rem;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.5);
  box-shadow: 0 0 0.8rem 0 rgba(0, 0, 0, 0.2);
  cursor: pointer;
}

.select {
  -webkit-appearance: none;
  appearance: none;
  display: flex;
  flex-direction: column;
  position: relative;
  width: 12rem;
  height: 2rem;
  margin: 1rem;
  box-shadow: 0 0 0.3rem 0 rgba(0, 0, 0, 0.2);
}

.option {
  padding: 0 2rem 0 0.7rem;
  min-height: 2rem;
  display: flex;
  align-items: center;
  background: #ffffff;
  border-top: #dddddd solid 1px;
  position: absolute;
  top: 0;
  width: 100%;
  pointer-events: none;
  order: 2;
  z-index: 11;
  transition: background 0.4s ease-in-out;
  box-sizing: border-box;
  overflow: hidden;
  white-space: nowrap;
}

.option:hover {
  background: #dddddd;
}

.select:focus .option {
  position: relative;
  pointer-events: all;
}

input {
  opacity: 0;
  position: absolute;
  left: -99999px;
}

input:checked + label {
  order: 1;
  z-index: 12;
  background: #ffffff;
  border-top: none;
  position: relative;
}

input:checked + label:after {
  content: "";
  width: 0;
  height: 0;
  border-left: 0.4rem solid transparent;
  border-right: 0.4rem solid transparent;
  border-top: 0.4rem solid #cccccc;
  position: absolute;
  right: 0.7rem;
  top: calc(50% - 0.16rem);
  pointer-events: none;
  z-index: 13;
}

input:checked + label:before {
  position: absolute;
  right: 0;
  height: 2rem;
  width: 2.2rem;
  content: "";
  background: #ffffff;
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
