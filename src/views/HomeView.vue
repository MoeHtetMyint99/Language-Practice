<script setup lang="ts">
import Header from '@/components/Header.vue'
import Button from '@/components/Button.vue'
import { inputText } from '@/assets/styles'
import { romanji, hiragana, katakana } from '@/assets/japWords'
import { computed, ref } from 'vue'

const numLearns = 15
const count = ref(10)
const randArr = ref<number[]>([])
const selected = ref('hiNka')
// const checkText = ref('')
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
  <div class="flex flex-col items-center">
    <div class="flex items-center gap-8 mb-4">
      <select v-model="selected" class="border p-2 rounded-md">
        <option>Romanji</option>
        <option value="romanjiCombined">Romanji (Hira + Kata)</option>
        <option>Hiragana</option>
        <option>Katakana</option>
        <option value="hiNka">Hiragana + Katakana</option>
      </select>
      <Button color="green" @click="onClick">Generate</Button>
      <input v-model="count" type="number" :class="inputText" class="w-fit max-w-20" />
      <Button color="green" @click="onCheck">Check</Button>
    </div>
    <div
      class="border-2 rounded-lg min-h-96 w-full text-green-300 bg-green-950 px-12 py-8 flex flex-col text-lg"
    >
      <div class="">{{ text }}</div>
      <div class="mt-8" v-if="checked">{{ checkText }}</div>
    </div>
  </div>
</template>
