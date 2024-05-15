<script></script>

<template>
  <div class="card">
    <div class="book-image-container">
      <img src="../assets/home2.jpg" class="book-no-image" />
    </div>
    <div class="author"></div>
    <div class="description">
      <div class="title"></div>
      <div class="caption"></div>
    </div>
    <div class="buy-container">
      <div class="buy-buttons"></div>
    </div>
  </div>
</template>

<style scoped>
.book-image-container {
  display: flex;
  align-items: center;
  justify-content: center;
}

.book-no-image {
  font-size: 4rem;
  font-weight: bold;
  display: block;
  transform: perspective(1500px) rotateY(20deg);
  box-shadow: rgba(0, 0, 0, 0.25) 0px 25px 50px -12px;
  transition: transform 1s ease 0s;
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
