<script setup lang="ts">
import Header from '@/components/Header.vue'
import Button from '@/components/Button.vue'
import { inputText } from '@/assets/styles'
import { romanji, hiragana, katakana, dakuonR, dakuonH, dakuonK } from '@/assets/japWords'
import { computed, ref } from 'vue'

const dakuonIncluded = ref(true)

const roman = computed(() => (dakuonIncluded.value ? romanji.concat(dakuonR) : romanji))
const hira = computed(() => (dakuonIncluded.value ? hiragana.concat(dakuonH) : hiragana))
const kata = computed(() => (dakuonIncluded.value ? katakana.concat(dakuonK) : katakana))

const wordsCount = computed(() => roman.value.length)
const count = ref(10)
const randArr = ref<number[]>([])
const selected = ref('hiNka')
const checked = ref(false)

const text = computed(() => {
  switch (selected.value) {
    case 'Romanji':
      return randArr.value.map((value) => roman.value[value]).join(' ')

    case 'romanjiCombined':
      return randArr.value
        .map((value) => {
          if (value > wordsCount.value) {
            return roman.value[value - wordsCount.value] + '(k)'
          } else {
            return roman.value[value]
          }
        })
        .join(' ')

    case 'Hiragana':
      return randArr.value.map((value) => hira.value[value]).join(' ')

    case 'Katakana':
      return randArr.value.map((value) => kata.value[value]).join(' ')

    case 'hiNka':
      const words = hira.value.concat(kata.value)
      return randArr.value.map((value) => words[value]).join(' ')
  }
})

const checkText = computed(() => {
  switch (selected.value) {
    case 'Romanji': {
      const arrCheckText = ['H : ']
        .concat(randArr.value.map((value) => hira.value[value]))
        .concat('<br>')
        .concat('K : ')
        .concat(randArr.value.map((value) => kata.value[value]))
      return arrCheckText.join(' ')
    }

    case 'romanjiCombined': {
      const arrCheckText = randArr.value.map((value) => {
        if (value >= wordsCount.value) {
          return kata.value[value - wordsCount.value]
        } else {
          return hira.value[value]
        }
      })
      return arrCheckText.join(' ')
    }

    case 'hiNka': {
      const arrCheckText = randArr.value.map((value) => {
        if (value >= wordsCount.value) {
          value = value - wordsCount.value
          return roman.value[value] + '(k)'
        } else {
          return roman.value[value]
        }
      })
      return arrCheckText.join(' ')
    }

    default: {
      const arrCheckText = randArr.value.map((value) => roman.value[value])
      return arrCheckText.join(' ')
    }
  }
})

const reset = () => {
  checked.value = false
  randArr.value = []
}

function onClick() {
  reset()

  if (selected.value === 'hiNka' || selected.value === 'romanjiCombined') {
    for (let i = 0; i < count.value; i++) {
      randArr.value.push(Math.floor(Math.random() * wordsCount.value * 2))
    }
  } else {
    for (let i = 0; i < count.value; i++) {
      randArr.value.push(Math.floor(Math.random() * wordsCount.value))
    }
  }
}

function onCheck() {
  checked.value = !checked.value
}
</script>

<template>
  <div class="flex flex-col items-center mt-4 lg:mt-0">
    <div class="flex items-center justify-evenly gap-2 mb-4 flex-wrap text-black">
      <select v-model="selected" class="border p-2 rounded-md" @change="reset">
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
      <input type="checkbox" class="" v-model="dakuonIncluded" @click="reset" />
    </div>
    <div
      class="border-2 rounded-lg min-h-96 w-full text-green-300 bg-green-950 px-12 py-8 flex flex-col text-lg"
    >
      <div class="">{{ text }}</div>
      <div class="mt-8" v-if="checked" v-html="checkText"></div>
      <!-- <div>{{ roman.length }}</div>
      <div>{{ hira.length }}</div>
      <div>{{ kata.length }}</div>
      <div>{{ ['5'].constructor === Array }}</div>
      <div>{{ typeof '5' }}</div> -->
    </div>
  </div>
</template>
