<script setup lang="ts">
import Header from '@/components/Header.vue'
</script>

<template>
  <div class="">
    <Header class="mb-8">About this page</Header>
    <div class="">
      <h1 class="text-lg">
        <span class="green">Generate</span> the japanese words randomly by
        <span class="green">selection</span> and <span class="green">numbers</span> of words to
        generate, and Practise!
        <div class="mt-4"><span class="green">Check</span> answer by Romanji.</div>
      </h1>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
