<template>
  <div class="card">
    <div class="book-image-container">
      <img :src="book.book_image" class="book-image" />
    </div>

    <div class="info">
      <div class="title">{{ book.title }}</div>
      <div class="author">{{ book.contributor }}</div>
      <div class="description">
        {{ book.description }}
      </div>
    </div>
    <div class="buy-buttons">
      <a v-for="link in book.buy_links" :href="link.url">
        <div class="link-icon">{{ link.name }}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  props: ["book"],
  data() {
    return {
      icons: {
        amazon: "assets/amazon.png",
      },
    };
  },
};
</script>

<style scoped>
.card {
  background-color: #ffffff;
  height: 30rem;
  width: 20rem;
  border: 2px solid rgba(0, 0, 0, 0.274);
  border-radius: 1rem;
  margin: 2rem;
  padding: 1rem;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
}

.book-image-container {
  display: flex;
  align-items: center;
  justify-content: center;
}

.book-image {
  max-height: 10rem;
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
}

.description {
  text-align: center;
  text-overflow: ellipsis;
}

/* Links styling */

.link-icon {
  width: 2rem;
  height: 2rem;
  margin: 0 0.4rem 0 0.4rem;
}
</style>
