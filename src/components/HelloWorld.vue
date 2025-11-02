<script setup>
import { ref, computed } from 'vue'

const choices = [
  { name: 'Rock', image: './src/assets/Piedra.jpg' },
  { name: 'Paper', image: './src/assets/Papel.png' },
  { name: 'Scissors', image: './src/assets/Tijeras.jpg' }
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
    <h1 class="text-3xl font-bold mb-6">Rock • Paper • Scissors</h1>

    <!-- Score -->
    <div class="flex justify-around mb-4 font-semibold">
      <div>Player: {{ score.player }}</div>
      <div>Computer: {{ score.computer }}</div>
      <div>Ties: {{ score.ties }}</div>
    </div>

    <!-- Choices -->
    <div class="flex justify-center gap-4 mb-4">
      <button v-for="c in choices" :key="c.name" @click="play(c)"
        class="p-2 rounded-lg border hover:scale-110 transition-transform">
        <img :src="c.image" :alt="c.name" class="w-20 h-20 object-contain" />
      </button>
    </div>

    <!-- Result -->
    <div v-if="player && computer" class="mb-4 flex justify-center gap-6 items-center">
      <div class="text-center">
        <p class="mb-1">You chose:</p>
        <img :src="player.image" :alt="player.name" class="w-24 h-24" />
      </div>
      <div class="text-center">
        <p class="mb-1">Computer chose:</p>
        <img :src="computer.image" :alt="computer.name" class="w-24 h-24" />
      </div>
    </div>

    <p v-if="result" class="text-2xl font-semibold mt-2">{{ resultMessage }}</p>
  </div>
</template>