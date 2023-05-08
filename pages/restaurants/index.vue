<template>
  <Title>Restaurant List | Top 10 Restaurant</Title>

  <div class="my-5 pt-5 text-center text-dark d-flex flex-column justify-content-center container">
    <h1 class="lh-lg fw-bolder">Top 10 Restaurant List</h1>
    <div class="row">
      <div class="col-md-6" v-for="item in restaurants" :key="item">
        <RestaurantCard :restaurant="item" />
      </div>
    </div>
  </div>
</template>


<script setup lang="ts">
import restaurants from "@/data.json";
</script>