<script setup lang="ts">
import { romanji, hiragana, katakana } from '@/assets/japWords'
</script>

<template>
  <div class="mt-10 lg:mt-0 w-full flex justify-center">
    <table id="words">
      <tr>
        <th>Romanji</th>
        <th>Hiragana</th>
        <th>Katakana</th>
      </tr>
      <tr v-for="(w, index) in romanji">
        <td>{{ romanji[index] }}</td>
        <td>{{ hiragana[index] }}</td>
        <td>{{ katakana[index] }}</td>
      </tr>
    </table>
  </div>
</template>

<style scoped>
#words {
  font-size: large;
}

#words th,
#words td {
  padding-inline: 8px;
  padding-block: 4px;
  text-align: center;
  border: 1px solid #ddd;
}

#words th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04aa6d;
  color: white;
  font-weight: bold;
}

@media (min-width: 1024px) {
  #words td {
    padding-block: 8px;
  }
}
</style>
