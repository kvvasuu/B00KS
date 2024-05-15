<template>
  <div class="header">
    <div class="input-container">
      <h2>Choose category</h2>
      <select class="select-category" name="categories">
        <option v-for="category in categories" value="category">
          {{ category }}
        </option>
      </select>
    </div>
  </div>
  <div class="items-container">
    <item-card></item-card>
    <item-card></item-card>
    <item-card></item-card>
    <item-card></item-card>
    <item-card></item-card>
    <item-card></item-card>
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
      categories: ["ab", "adsa", "rtyetre", "dsade"],
    };
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
}
</style>
