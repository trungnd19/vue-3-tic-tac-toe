<script setup lang="ts">
import { ref, computed, Ref } from "vue";

const player: Ref<string> = ref("X");
const board: Ref<string[][]> = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""]
]);
const winner = computed(() => calculateWinner(board.value.flat()));
const draw = computed(() => {
  const boardValue = board.value.flat().every((cell) => cell);
  return !calculateWinner(board.value.flat()) && boardValue;
});

function calculateWinner(flattenBoard: string[]): string | null {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];

    if (flattenBoard[a] && flattenBoard[a] === flattenBoard[b] && flattenBoard[a] === flattenBoard[c]) {
      return flattenBoard[a];
    }
  }

  return null;
}

// x: hàng; y: ô trong hàng
function makeMove(x: number, y: number): void {
  if (winner.value) {
    return; // the game is over
  }

  if (board.value[x][y] !== "") {
    return; // cannot register a move in a position that has beeen marked before
  }

  board.value[x][y] = player.value;
  changePlayerTurn();
}

function resetGame(): void {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""]
  ];
  player.value = "X";
}

function changePlayerTurn(): void {
  if (player.value === "X") {
    player.value = "O";
  } else {
    player.value = "X";
  }
}
</script>

<template>
  <main class="pt-8 text-center bg-white text-black dark:text-white">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>

    <h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex border border-black">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="makeMove(x, y)"
          :class="`border border-black w-24 h-24 hover:bg-green-200 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
            cell === 'X' ? 'text-pink-500' : 'text-blue-400'
          }`"
        >
          <!-- "close"/ "circle" is material icon -->
          {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
        </div>
      </div>
    </div>

    <div class="text-center">
      <h2 v-if="winner" class="text-6xl font-bold mb-8">
        Player '{{ winner }}' wins!
      </h2>
      <h2 class="text-6xl font-bold mb-8" v-if="draw">Draw!</h2>
      <button
        @click="resetGame"
        class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-600 duration-300"
      >
        Reset
      </button>
    </div>
  </main>
</template>

<style scoped>
body {
  @apply bg-gray-800 text-white;
}
</style>
