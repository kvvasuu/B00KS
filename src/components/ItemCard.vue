<template>
  <div class="card">
    <div class="book-image-container">
      <img :src="book.book_image" class="book-no-image" />
    </div>
    <div class="author">{{ book.author }}</div>
    <div class="description">
      <div class="title">{{ book.title }}</div>
      <div class="caption">
        {{ book.description }}
      </div>
    </div>
    <div class="buy-container">
      <div class="buy-buttons"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["book"],
  data() {
    return {};
  },
};
</script>

<style scoped>
.book-image-container {
  display: flex;
  align-items: center;
  justify-content: center;
}

.card {
  background-color: #ffffff;
  height: 30rem;
  width: 20rem;
  border: 2px solid rgba(0, 0, 0, 0.274);
  border-radius: 1rem;
  margin: 2rem;
}
</style>
