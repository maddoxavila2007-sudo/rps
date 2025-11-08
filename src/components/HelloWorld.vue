<script setup>
import { ref, computed } from 'vue'

const choices = [
 {
    name: 'Rock',
    svg: `<svg xmlns="http://www.w3.org/2000/svg&quot; fill="none" viewBox="0 0 64 64">
      <path fill="#888" d="M20 10h24v44H20z" />
      <path fill="#555" d="M16 26h32v28H16z" />
    </svg>`
  },
  {
    name: 'Paper',
    svg: `<svg xmlns="http://www.w3.org/2000/svg&quot; fill="none" viewBox="0 0 64 64">
      <rect x="16" y="8" width="32" height="48" fill="#ddd" stroke="#999" stroke-width="2" />
      <line x1="22" y1="16" x2="42" y2="16" stroke="#999" stroke-width="2"/>
      <line x1="22" y1="22" x2="42" y2="22" stroke="#999" stroke-width="2"/>
      <line x1="22" y1="28" x2="42" y2="28" stroke="#999" stroke-width="2"/>
    </svg>`
  },
  {
    name: 'Scissors',
    svg: `<svg xmlns="http://www.w3.org/2000/svg&quot; fill="none" viewBox="0 0 64 64">
      <circle cx="20" cy="20" r="8" stroke="#555" stroke-width="3"/>
      <circle cx="44" cy="20" r="8" stroke="#555" stroke-width="3"/>
      <line x1="20" y1="28" x2="44" y2="44" stroke="#555" stroke-width="3"/>
      <line x1="20" y1="44" x2="44" y2="28" stroke="#555" stroke-width="3"/>
    </svg>`
  }
]

const player = ref(null)
const computer = ref(null)
const result = ref(null)

const score = ref({
  player: 0,
  computer: 0,
  ties: 0
})

function play(choice) {
  player.value = choice
  computer.value = choices[Math.floor(Math.random() * choices.length)]
  result.value = decideWinner(player.value.name, computer.value.name)

  if (result.value === 'win') score.value.player++
  else if (result.value === 'lose') score.value.computer++
  else score.value.ties++
}

function decideWinner(p, c) {
  if (p === c) return 'tie'
  if (
    (p === 'Rock' && c === 'Scissors') ||
    (p === 'Paper' && c === 'Rock') ||
    (p === 'Scissors' && c === 'Paper')
  ) {
    return 'win'
  }
  return 'lose'
}

const resultMessage = computed(() => {
  if (!result.value) return ''
  if (result.value === 'win') return 'You Win! 🎉'
  if (result.value === 'lose') return 'You Lose! 😞'
  return "It's a Tie! 🤝"
})
</script>

<template>
  <div class="max-w-lg mx-auto p-6 bg-white rounded-xl shadow-md text-center">
    <h1 class="text-3xl font-bold mb-6"> Rock • Paper • Scissors </h1>

    <!-- Score -->
    <div class="flex justify-around mb-4 font-semibold">
      <div>Player: {{ score.player }}</div>
      <div>Computer: {{ score.computer }}</div>
      <div>Ties: {{ score.ties }}</div>
    </div>

    <!-- Choices -->
    <div class="flex justify-center gap-4 mb-4">
      <button 
      v-for="c in choices" 
      :key="c.name" 
      @click="play(c)"
        class="p-2 rounded-lg border hover:scale-110 transition-transform">

        <div v-html="c.svg" class="w-16 h-16"></div>
      </button>
    </div>

    <!-- Result -->
    <div v-if="player && computer" class="mb-4 flex justify-center gap-6 items-center">
      <div class="text-center">
        <p class="mb-1">You chose:</p>
        <div v-html="player.svg" class="w-24 h-24 mx-auto"></div>
      </div>
      
      <div class="text-center">
        <p class="mb-1">Computer chose:</p>
        <div v-html="computer.svg" class="w-24 h-24 mx-auto"></div>
    </div>

    <p v-if="result" class="text-2xl font-semibold mt-2">{{ resultMessage }}</p>

    <button class="deletebtn" @click="score.player = 0; score.computer = 0; score.ties = 0;">
      Delete Score
    </button>

    <button class="resetbtn" @click="player = null; computer = null; result = null;">
      Reset Game
    </button>

  </div>
  </div>
</template>