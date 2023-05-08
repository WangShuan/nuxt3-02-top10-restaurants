<template>
  <Title>Home | Top 10 Restaurants</Title>

  <div
    class="bg-image vh-100 text-center text-dark d-flex flex-column justify-content-center align-items-center position-relative p-3">
    <h1 class="fw-bolder">Welcome To Top 10 Restaurants</h1>
    <NuxtLink class="link-success fw-bold fs-4" to="/restaurants">👉 Go To See The Top 10 Restaurant List</NuxtLink>
  </div>
</template>


<style scoped>
.bg-image {
  background-image: url("https://images.unsplash.com/photo-1505935428862-770b6f24f629?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2367&q=80");
  background-size: cover;
  background-position: center;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>