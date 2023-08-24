<script setup lang="ts">
import { RouterView } from 'vue-router'
</script>

<template>
  <div class="min-h-screen bg-sky-100">
    <header>
        <h1 class="text-center text-5xl pt-4">Weather App</h1>
    </header>

    <!-- Used for SPA application the route is displayed inside the routerview -->
    <RouterView />
  </div>

</template>