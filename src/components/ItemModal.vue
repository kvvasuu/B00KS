<template>
  <div @click="() => this.$emit('toggle-modal')" class="item-modal">
    <div class="item-modal-content" @click.stop="">
      <img :src="book.book_image" class="book-image" :title="book.title" />
    </div>
  </div>
</template>

<script>
export default {
  props: ["book"],
  data() {
    return {};
  },
  methods: {
    getLinkURL(name) {
      return `/${name.toLowerCase().split(" ").join("-")}.png`;
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
.item-modal {
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 99;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
}

.item-modal-content {
  position: inherit;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  box-shadow: 0 0 0.625rem rgba(0, 0, 0, 0.3);
}

.book-image {
  height: 24rem;
}
</style>
