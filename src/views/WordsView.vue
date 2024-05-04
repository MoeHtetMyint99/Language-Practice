<script setup lang="ts">
import { romanji, hiragana, katakana, dakuonR, dakuonH, dakuonK } from '@/assets/japWords'
</script>

<template>
  <div class="mt-10 pb-8 lg:mt-0 w-full flex flex-col items-center justify-center">
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
    <div class="mt-8 mb-4 underline underline-offset-4 text-lg text-green-400">
      Dakuon & Handakuon
    </div>
    <table id="words">
      <tr>
        <th>Romanji</th>
        <th>Hiragana</th>
        <th>Katakana</th>
      </tr>
      <tr v-for="(w, index) in dakuonR">
        <td>{{ dakuonR[index] }}</td>
        <td>{{ dakuonH[index] }}</td>
        <td>{{ dakuonK[index] }}</td>
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
  text-align: center;
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
