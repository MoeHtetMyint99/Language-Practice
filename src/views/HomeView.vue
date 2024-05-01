<script setup lang="ts">
import Header from '@/components/Header.vue'
import Button from '@/components/Button.vue'
import { inputText } from '@/assets/styles'
import { romanji, hiragana, katakana } from '@/assets/japWords'
import { computed, ref } from 'vue'

const numLearns = 25
const count = ref(10)
const randArr = ref<number[]>([])
const selected = ref('hiNka')
const checked = ref(false)

const text = computed(() => {
  switch (selected.value) {
    case 'Romanji':
      return randArr.value.map((value) => romanji[value]).join(' ')

    case 'romanjiCombined':
      return randArr.value
        .map((value) => {
          if (value > numLearns) {
            return romanji[value - numLearns] + '(k)'
          } else {
            return romanji[value]
          }
        })
        .join(' ')

    case 'Hiragana':
      return randArr.value.map((value) => hiragana[value]).join(' ')

    case 'Katakana':
      return randArr.value.map((value) => katakana[value]).join(' ')

    case 'hiNka':
      const words = hiragana.concat(katakana)
      return randArr.value.map((value) => words[value]).join(' ')
  }
})

const checkText = computed(() => {
  if (selected.value === 'hiNka') {
    const arrCheckText = randArr.value.map((value) => {
      value = value > numLearns ? value - numLearns : value
      return romanji[value]
    })
    return arrCheckText.join(' ')
  } else {
    const arrCheckText = randArr.value.map((value) => romanji[value])
    return arrCheckText.join(' ')
  }
})

function onClick() {
  checked.value = false
  randArr.value = []

  if (selected.value === 'hiNka' || selected.value === 'romanjiCombined') {
    for (let i = 0; i < count.value; i++) {
      randArr.value.push(Math.floor(Math.random() * numLearns * 2))
    }
  } else {
    for (let i = 0; i < count.value; i++) {
      randArr.value.push(Math.floor(Math.random() * numLearns))
    }
  }
}

function onCheck() {
  checked.value = !checked.value
}
</script>

<template>
  <div class="flex flex-col items-center mt-4 lg:mt-0">
    <div class="flex items-center justify-evenly gap-2 mb-4 flex-wrap">
      <select v-model="selected" class="border p-2 rounded-md">
        <option>Romanji</option>
        <option value="romanjiCombined">Romanji (Hira + Kata)</option>
        <option>Hiragana</option>
        <option>Katakana</option>
        <option value="hiNka">Hiragana + Katakana</option>
      </select>
      <input v-model="count" type="number" :class="inputText" class="" />
      <Button color="green" @click="onClick">Generate</Button>
      <Button color="green" class="w-[5rem] px-0 flex justify-center" @click="onCheck">{{
        checked ? 'Uncheck' : 'Check'
      }}</Button>
    </div>
    <div
      class="border-2 rounded-lg min-h-96 w-full text-green-300 bg-green-950 px-12 py-8 flex flex-col text-lg"
    >
      <div class="">{{ text }}</div>
      <div class="mt-8" v-if="checked">{{ checkText }}</div>
      <!-- <div>{{ romanji.length }}</div>
      <div>{{ hiragana.length }}</div>
      <div>{{ katakana.length }}</div> -->
    </div>
  </div>
</template>
