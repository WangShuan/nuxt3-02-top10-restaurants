<template>
  <NuxtLayout>
    <div
      class="bg-opacity-25 bg-success vh-100 text-center text-dark d-flex flex-column justify-content-center align-items-center p-3">
      <h1 class="fw-bolder">{{ err.statusCode }}</h1>
      <p>{{ err.message }}</p>
      <button @click="handleError()" class="btn btn-outline-success fw-bold" to="/restaurants">Go Home</button>
    </div>
  </NuxtLayout>
</template>

<script setup>
const err = useError();

const handleError = () => clearError({ redirect: '/' })
</script>
