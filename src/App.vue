<script setup>
import { ref, computed } from "vue";
const clickedSquares = ref([]);
const chessboard = ref(generateChessboard());
function generateChessboard() {
  const rows = 8;
  const cols = 8;
  const chessboard = [];
  const letters = ["A", "B", "C", "D", "E", "F", "G", "H"];

  for (let i = 0; i < cols; i++) {
    const column = [];
    for (let j = 0; j < rows; j++) {
      column.push({ id: `${letters[i]}${8 - j}`, clicked: false });
    }
    chessboard.push(column);
  }
  console.log(chessboard);
  return chessboard;
}

function handleSquareClick(rowIndex, colIndex) {
  const square = chessboard.value[rowIndex][colIndex];
  clickedSquares.value.push(square.id);

  square.clicked = true;
}

const isOdd = computed(() => {
  return (row, col) => (row + col) % 2 !== 0;
});
</script>
<template>
  <div class="chessboard">
    <div class="chessboard__container">
      <div
        class="chessboard__column"
        v-for="(row, rowIndex) in chessboard"
        :key="rowIndex"
      >
        <div
          class="chessboard__square"
          v-for="(square, colIndex) in row"
          :key="colIndex"
          @click="handleSquareClick(rowIndex, colIndex)"
          :class="{
            clicked: square.clicked,
            odd: !isOdd(rowIndex, colIndex),
            even: isOdd(rowIndex, colIndex),
          }"
        ></div>
      </div>
    </div>
    <div class="chessboard__sidebar">
      <h2>Clicked Squares</h2>
      <ul>
        <li v-for="(square, index) in clickedSquares" :key="index">
          {{ square }}
        </li>
      </ul>
    </div>
  </div>
  {{ letters }}
</template>

<style lang="scss">
.chessboard {
  display: flex;
  flex-direction: column;

  @media (min-width: 768px) {
    flex-direction: row;
    max-width: 500px;
  }
  &__container {
    flex-grow: 1;
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    grid-template-rows: repeat(8, 1fr);
    height: 500px;
    @media (max-width: 468px) {
      height: 300px;
    }
  }
  &__sidebar {
    flex-shrink: 0;
    width: 200px;
  }
  &__square {
    padding: 1.85rem;
    @media (min-width: 768px) {
      padding: 40px;
    }
    @media (max-width: 468px) {
      padding: 1.25rem;
    }
    &.odd {
      background-color: #e9edcc;
    }
    &.even {
      background-color: #779954;
    }
    &.clicked {
      background-color: yellow;
    }
  }
}
</style>
