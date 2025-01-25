<script setup lang="ts">
import Banner from "../components/Home/Banner.vue";
import Card from "../components/Home/Card.vue";
</script>

<template>
  <div class="home">
    <Banner />
    <div class="center heading">
      <h1>What is WizFit Challenge?</h1>
      <button class="watch center">
        <span class="triangle"></span>Watch Video
      </button>
    </div>
    <Card />
  </div>
</template>

<style scoped>
.heading {
  gap: 20px;
  padding: 0 20px;
}

.watch {
  background: #d52c50;
  color: white;
  padding: 10px 20px;
  font-size: 15px;
}

h1 {
  color: #d1224e;
}

.triangle {
  display: inline-block;
  width: 0;
  height: 0;
  border: solid 12px;
  border-color: transparent transparent white transparent;
  transform: rotate(90deg);
}

@media (max-width: 640px) {
  .heading {
    flex-direction: column;
  }
}
</style>
