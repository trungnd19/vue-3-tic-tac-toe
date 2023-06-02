<script setup lang="ts">
import { ref, computed, Ref, ComputedRef } from "vue";

const player: Ref<string> = ref("X");
const board: Ref<string[][]> = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""]
]);
const winner: ComputedRef<string[] | null> = computed(() =>
  calculateWinner(board.value.flat())
);

function calculateWinner(board: string[]): string | null {
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

    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
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
  <h1>Hello World!</h1>
</template>

<style scoped></style>
