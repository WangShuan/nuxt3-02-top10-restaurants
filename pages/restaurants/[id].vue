<template>
  <Title>Top{{ restaurant.rank }} {{ restaurant.name }} | Top 10 Restaurant</Title>
  <div class="container my-5 pt-5">
    <div class="row g-0 justify-content-center">
      <div class="col-xl-8">
        <img class="img-fluid w-100" :src="restaurant.imageUrl" alt="">
      </div>
      <div class="col-xl-8 p-3 p-md-5 bg-light d-flex flex-column justify-content-between">
        <div class="d-flex flex-column align-items-start">
          <small class="badge rounded-pill text-bg-danger">TOP {{ restaurant.rank }}</small>
          <h1 class="text-success fw-bolder my-2 fs-3">
            {{ restaurant.name }}
          </h1>
        </div>
        <p class="lh-lg my-3 me-0 me-xl-5">{{ restaurant.content }}</p>
        <span>
          全球分店數量：{{ restaurant.numberOfStores }}+
        </span>
      </div>
    </div>
  </div>
  <div class="bg-success text-center p-3 p-md-5 my-5">
    <h2 class="fs-3 py-3"><a class="link-warning" :href="`https://www.google.com/search?q=${restaurant.name}`"
        target="_blank">👉 GO TO {{ restaurant.name }} WEBSITE</a>
    </h2>
  </div>
  <div class="container my-5 d-flex justify-content-between">
    <NuxtLink class="btn btn-success rounded-pill" :class="{ 'disabled': currentId === 1 }"
      :to="`/restaurants/${currentId - 1}`">
      ＜ 上一家餐廳 </NuxtLink>
    <NuxtLink class="btn btn-success rounded-pill" :class="{ 'disabled': currentId === 10 }"
      :to="`/restaurants/${currentId + 1}`">下一家餐廳 ＞
    </NuxtLink>
  </div>
</template>

<script setup lang="ts">
import restaurants from "@/data.json";
interface Restaurant {
  id: number;
  rank: number;
  name: string;
  content: string;
  numberOfStores: string;
  imageUrl: string;
}
const currentId = Number(useRoute().params.id);
const restaurant = restaurants.find(item => item.id === currentId) as Restaurant;
if (!restaurant) {
  throw createError({
    statusCode: 404,
    message: "This ID not found, please try again."
  })
}
</script>


<style scoped>
.bg-image {
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}
</style>