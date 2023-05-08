<template>
  <NuxtLink class="text-decoration-none" :to="`/restaurants/${restaurant.id}`"
    :class="restaurant.rank > 3 ? 'link-success' : 'link-danger'">
    <div class="card my-3 overflow-hidden">
      <div class="ratio ratio-16x9 card-img">
        <img :src="restaurant.imageUrl" class="img-fluid" alt="...">
      </div>
      <div class="bg-white-5 card-img-overlay h-100 d-flex align-items-center justify-content-center">
        <div class=" p-3">
          <span class="fw-bolder rounded-pill badge" style="font-size: 14px;"
            :class="restaurant.rank > 3 ? 'text-bg-success' : 'text-bg-danger'">
            Top {{ restaurant.rank }}
          </span>
          <h2 class="fs-5 fw-bolder px-2 py-1 m-0 text-shadow">
            {{ restaurant.name }}
          </h2>
        </div>
      </div>
    </div>
  </NuxtLink>
</template>

<script setup lang="ts">
interface Restaurant {
  restaurant: {
    id: number;
    rank: number;
    name: string;
    content: string;
    revenue: number;
    numberOfStores: string;
    imageUrl: string;
  }
}
const props = defineProps<Restaurant>()
</script>

<style>
.text-shadow {
  text-shadow: black 0.1em 0.1em 0.2em
}

.bg-white-5 {
  background: rgba(0, 0, 0, 0.25);
}
</style>