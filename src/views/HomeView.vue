<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue"

// ========== Life Cycle Section ============

onMounted(() => {
  initializeGameBoard();
  window.addEventListener('keydown', handleKeydown);
});

onBeforeUnmount(() => {
  window.addEventListener('keydown', handleKeydown);
})

// ========== Variables Section ============

const selectedBlock = ref(null)
const rows = ref(5)
const cols = ref(4)
const containerWidthInRem = ref(69)
const totalCells = ref([]);

let blocks = ref([
  { id: 'A', xAxis: 0, yAxis: 0, width: 2, height: 2, message: "Introduction" },      // 2x2
  { id: 'B', xAxis: 0, yAxis: 2, width: 2, height: 1, message: "Skills" },            // 2x1
  { id: 'C', xAxis: 2, yAxis: 0, width: 2, height: 1, message: "Work Exp" },          // 1x2
  { id: 'D', xAxis: 2, yAxis: 1, width: 2, height: 1, message: "Educ Background" },   // 2x1
  { id: 'E', xAxis: 2, yAxis: 2, width: 2, height: 1, message: "Projects" },          // 2x1
  { id: 'F', xAxis: 0, yAxis: 3, width: 1, height: 1, message: "Contact" },           // 1x1
  { id: 'G', xAxis: 1, yAxis: 3, width: 1, height: 1, message: "Pic" },               // 1x1
  { id: 'H', xAxis: 2, yAxis: 3, width: 1, height: 1, message: "Hello" },             // 1x1
  { id: 'I', xAxis: 3, yAxis: 3, width: 1, height: 1, message: "Pic" }                // 1x1
])

// ========== Event Listeners ==============

const handleKeydown = (e) => {
  if (!selectedBlock.value) return

  const currentBlock = selectedBlock.value
  const maxColumns = rows.value
  const maxRows = cols.value

  switch (e.key) {
    case 'ArrowRight':
      // Check if the block's right edge stays within the grid
      if (currentBlock.xAxis + currentBlock.width < maxColumns) {
        moveRight(currentBlock);
      }
      break
    case 'ArrowLeft':
      // Check if the block's left edge stays within the grid
      if (currentBlock.xAxis > 0) {
        moveLeft(currentBlock);
      }
      break
    case 'ArrowUp':
      // Check if the block's top edge stays within the grid
      if (currentBlock.yAxis > 0) {
        moveUp(currentBlock);
      }
      break
    case 'ArrowDown':
      // Check if the block's bottom edge stays within the grid
      if (currentBlock.yAxis + currentBlock.height < maxRows) {
        moveDown(currentBlock);
      }
      break
  }
}


// Functions Section

// Initialize the game board
const initializeGameBoard = () => {
  // Clear the grid first if necessary
  totalCells.value = [];

  for (let y = 0; y < rows.value; y++) {
    for (let x = 0; x < cols.value; x++) {
      totalCells.value.push({ x, y, block: null });
    }
  }
};

const selectBlockToMove = (block) => {
  selectedBlock.value = null
  selectedBlock.value = block
  console.log(selectedBlock.value)
}

function isCollision(nextX, nextY, w, h, currentId) {
  for (const other of blocks.value) {
    if (other.id === currentId) continue;

    const overlapX = nextX < other.xAxis + other.width && nextX + w > other.xAxis;
    const overlapY = nextY < other.yAxis + other.height && nextY + h > other.yAxis;

    if (overlapX && overlapY) return true;
  }
  return false;
}

function moveRight(block) {
  const nextX = block.xAxis + 1;
  const nextY = block.yAxis;
  if (!isCollision(nextX, nextY, block.width, block.height, block.id)) {
    block.xAxis = nextX;
  }
}

function moveLeft(block) {
  const nextX = block.xAxis - 1;
  const nextY = block.yAxis;
  if (!isCollision(nextX, nextY, block.width, block.height, block.id)) {
    block.xAxis = nextX;
  }
}

function moveUp(block) {
  const nextX = block.xAxis;
  const nextY = block.yAxis - 1;
  if (!isCollision(nextX, nextY, block.width, block.height, block.id)) {
    block.yAxis = nextY;
  }
}

function moveDown(block) {
  const nextX = block.xAxis;
  const nextY = block.yAxis + 1;
  if (!isCollision(nextX, nextY, block.width, block.height, block.id)) {
    block.yAxis = nextY;
  }
}

</script>

<template>

  <main class="flex flex-col w-screen h-screen bg-[#fefae0] center-all">

    <!-- Portfolio Header -->
    <div class="flex items-center w-[69rem] h-[4rem] mb-2 bg-slate-300">

      <div class="center-all bg-slate-500 h-[3rem] w-[3rem] rounded-md font-bold m-2 text-white cursor-pointer">JD</div>

      <div class="flex flex-col">
        <div class="ml-3 mb-2 flex items-center cursor-pointer">
          <p class="text-[1.15rem] font-semibold">Game Mode </p>
        </div>
        <div class="bg-slate-500 border-b-4 border-slate-500 ml-3 w-full"></div>
      </div>

      <div class="bg-slate-500 ml-auto mr-4 px-4 py-2 text-white rounded">L and D mode</div>

    </div>


    <!-- Container for grid and blocks -->
    <div :class="`relative w-[${containerWidthInRem}rem]`">

      <!-- Grid Wrapper -->
      <div class="grid grid-cols-5 bg-[#f5deb3] border border-slate-500 ">
        <div v-for="(cell, index) in totalCells" :key="`${cell.x}-${cell.y}`"
          class="w-[220px] h-[195px]  border border-white text-xs center-all ">
          <!-- index {{ index }} -->
        </div>
      </div>

      <!-- Blocks Rendering (absolute within relative container) -->
      <div v-for="block in blocks" :key="block.id" @click="selectBlockToMove(block)" :class="[
        'absolute text-[#5c4033] text-[1.5rem] flex center-all cursor-pointer border border-[#7a5b3c] hover:bg-slate-500',
        block.id === 'A'
          ? 'bg-[#b08968] hover:bg-[#a07556]'
          : 'bg-[#d2b48c] hover:bg-[#c4a67c]'
      ]" :style="{
        left: `${block.xAxis * 221}px`,
        top: `${block.yAxis * 195}px`,
        width: `${block.width * 221}px`,
        height: `${block.height * 195}px`,
      }">
        {{ block.message }}
        <!-- {{ block.id }} - ( x : {{ block.xAxis }}, y : {{ block.yAxis }} ) -->

      </div>

    </div>

  </main>


</template>
