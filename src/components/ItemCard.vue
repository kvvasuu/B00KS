<template>
  <div class="card">
    <div class="rank">{{ book.rank }}</div>
    <div class="weeks-on-list">{{ weeksOnList }} weeks <br />on list</div>
    <div class="book-image-container" @click="itemModalHandle">
      <img :src="book.book_image" class="book-image" :title="book.title" />
    </div>

    <div class="info">
      <div class="title">{{ book.title }}</div>
      <div class="author">{{ book.contributor }}</div>
      <div class="description">
        {{ book.description }}
      </div>
    </div>
    <div class="buy-buttons">
      <div class="buy-caption">Buy</div>
      <a v-for="link in links" :href="link.url" target="_blank">
        <img
          :src="getLinkURL(link.name)"
          class="link-icon"
          :title="link.name"
        />
      </a>
    </div>
  </div>
  <transition name="fade">
    <item-modal
      v-if="itemModalShow"
      @toggle-modal="itemModalHandle"
      :book="book"
    ></item-modal>
  </transition>
</template>

<script>
import ItemModal from "./ItemModal.vue";

export default {
  props: ["book"],
  components: {
    ItemModal,
  },
  data() {
    return { itemModalShow: false };
  },
  methods: {
    getLinkURL(name) {
      return `/${name.toLowerCase().split(" ").join("-")}.png`;
    },
    itemModalHandle() {
      this.itemModalShow = !this.itemModalShow;
    },
  },
  computed: {
    links() {
      return this.book.buy_links.slice(0, 5);
    },
    weeksOnList() {
      return this.book.weeks_on_list || `< ${this.book.weeks_on_list + 1}`;
    },
  },
};
</script>

<style scoped>
.card {
  position: relative;
  height: 30rem;
  width: 20rem;
  margin: 2rem;
  padding: 1rem;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
}

.card::after {
  transition: opacity 0.3s ease-in-out;
  opacity: 0;
  border-radius: 1.4rem;
  position: absolute;
  width: 20rem;
  height: 23rem;
  z-index: -2;
  bottom: 0%;
  content: "";
  box-shadow: 0 1rem 1rem rgba(0, 0, 0, 0.3);
  background: rgb(9, 177, 199);
}

.card:hover::after {
  opacity: 1;
}

.book-image-container {
  cursor: pointer;
  position: relative;
}

.book-image-container:after {
  /* book shadow */
  position: absolute;
  width: 80%;
  height: 10%;
  top: 50%;
  left: 10%;
  background-color: transparent;
  border-radius: 100px;
  z-index: -1;
  bottom: 0%;
  content: "";
  box-shadow: 0 5.6rem 1.5rem rgba(0, 0, 0, 0.3);
}

.rank {
  font-size: 3rem;
  position: absolute;
  top: 0;
  left: 0.4rem;
  border-radius: 100px;
  z-index: 2;
  filter: drop-shadow(0 1px 1px rgba(0, 0, 0, 0.3));
  color: rgb(9, 177, 199);
  font-family: Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;
}

.weeks-on-list {
  font-size: 0.8rem;
  position: absolute;
  top: 3.8rem;
  left: 0.4rem;
  border-radius: 100px;
  z-index: 2;
  filter: drop-shadow(0 1px 1px rgba(0, 0, 0, 0.3));
}

.card:hover .book-image {
  translate: 0 -3px;
}

.book-image {
  max-height: 10rem;
  transition: all 0.3s ease-in-out;
}

.info {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.title {
  text-align: center;
  margin: 1rem 1rem 0 1rem;
  font-size: 1.1rem;
  font-weight: bolder;
}

.author {
  font-size: 0.9rem;
  color: #242424be;
  text-align: center;
}

.description {
  text-align: center;
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}

.link-icon {
  height: 2rem;
  margin: 0 0.5rem 0 0.5rem;
  border-radius: 0.3rem;
  filter: drop-shadow(0 0.2rem 0.3rem rgba(0, 0, 0, 0.3));
}
.buy-caption {
  font-size: 0.8rem;
  color: #242424be;
  text-align: center;
  margin-bottom: 0.5rem;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
  animation: blur-in 0.5s ease-in-out forwards;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  animation: blur-out 0.5s ease-in-out forwards;
}
</style>
