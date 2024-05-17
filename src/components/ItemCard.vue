<template>
  <div class="card">
    <div class="book-image-container">
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
  /* border-top-left-radius: 0;
  border-top-right-radius: 0; */
  position: absolute;
  width: 20rem;
  height: 24rem;
  z-index: -2;
  bottom: 0%;
  content: "";
  box-shadow: 0 1rem 1rem rgba(0, 0, 0, 0.3);
  background: rgb(9, 177, 199);
  /* background: linear-gradient(
    0deg,
    rgba(9, 177, 199, 1) 70%,
    rgba(180, 117, 44, 0) 100%
  ); */
}

.card:hover::after {
  opacity: 1;
}

.book-image-container {
  position: relative;
}

.book-image-container:after {
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

.card:hover .book-image {
  translate: 0 -3px;
  mix-blend-mode: normal;
}

.book-image {
  max-height: 10rem;
  transition: all 0.3s ease-in-out;
  mix-blend-mode: multiply;
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
</style>
