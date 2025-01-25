<script setup lang="ts"></script>

<template>
  <div class="banner">
    <img alt="Banner" src="../../assets/home/banner.png" />
  </div>
</template>

<style scoped>
.banner {
  /* border: 1px solid #ccc; */
  width: 100%;
}
.banner img {
  width: 100%;
}

@media (max-width: 640px) {
  .banner {
    height: 100px;
  }
}
</style>
