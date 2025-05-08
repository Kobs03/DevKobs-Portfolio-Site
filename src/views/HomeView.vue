<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue"
import NET from 'vanta/dist/vanta.net.min'
import * as THREE from 'three'

// ========== Life Cycle Section ============

const vantaRef = ref(null)
let vantaEffect = null

onMounted(() => {
  vantaEffect = NET({
    el: vantaRef.value,
    THREE,
    color: 0x888888,
    backgroundColor: 0x1e1e2f,   // <-- background
    points: 10.0,
    maxDistance: 15.0,
    spacing: 20.0
  })
  initializeGameBoard();
  window.addEventListener('keydown', handleKeydown);
});

onBeforeUnmount(() => {
  window.addEventListener('keydown', handleKeydown);
  if (vantaEffect) vantaEffect.destroy()
})

// ========== Variables Section ============

const selectedBlock = ref(null)
const rows = ref(5)
const cols = ref(4)
const containerWidthInRem = ref(69)
const totalCells = ref([]);

let blocks = ref([
  { id: 'A', xAxis: 0, yAxis: 0, width: 2, height: 2, message: "Jake Dedicatoria" },  // 2x2
  { id: 'B', xAxis: 0, yAxis: 2, width: 2, height: 1, message: "Skills" },            // 2x1
  { id: 'C', xAxis: 2, yAxis: 0, width: 2, height: 1, message: "Work Experience" },          // 1x2
  { id: 'D', xAxis: 2, yAxis: 1, width: 2, height: 1, message: "Education" },         // 2x1
  { id: 'E', xAxis: 2, yAxis: 2, width: 2, height: 1, message: "Projects" },          // 2x1
  { id: 'F', xAxis: 0, yAxis: 3, width: 1, height: 1, message: "Contact" },           // 1x1
  { id: 'G', xAxis: 1, yAxis: 3, width: 1, height: 1, message: "Socials" },               // 1x1
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

  <div ref="vantaRef" class="relative w-screen h-screen">

    <div class="relative z-10 text-white text-center h-full center-all">

      <main class="flex flex-col center-all w-full h-full">

        <!-- Portfolio Header -->
        <div
          class="flex items-center w-[69rem] h-[4rem] mb-2 bg-white/10 backdrop-blur-md border border-white/20 rounded-xl shadow-md">

          <div class="center-all bg-slate-500 h-[3rem] w-[3rem] rounded-md font-bold m-2 text-white cursor-pointer">JD
          </div>

          <div class="flex flex-col text-[#4F4F4F]">
            <div class="ml-3 mb-1 flex items-center cursor-pointer">
              <p class="text-[1.15rem] text-white font-semibold"> Home </p>
            </div>
            <div class="bg-slate-500 border-b-4 border-slate-500 ml-3 w-full"></div>
          </div>

          <!-- <div class="bg-slate-500 ml-auto mr-4 px-4 py-2 text-white rounded">L and D mode</div> -->

        </div>


        <!-- Container for grid and blocks -->
        <div :class="`relative w-[${containerWidthInRem}rem]`">
          <!-- Grid Wrapper -->
          <div class="grid grid-cols-5 bg-white/10 backdrop-blur-md border border-white/20 ">
            <div v-for="(cell, index) in totalCells" :key="`${cell.x}-${cell.y}`"
              class="w-[220px] h-[195px] border border-[#8D8D8D] text-xs center-all ">
              <!-- index {{ index }} -->
            </div>
          </div>

          <!-- Blocks Rendering (absolute within relative container) -->
          <div v-for="block in blocks" :key="block.id" @click="selectBlockToMove(block)" :class="[
            'absolute text-white text-[1.5rem] flex flex-col justify-end cursor-pointer bg-white/10 backdrop-blur-md border border-white/50',
            block.id === 'A'
              ? 'bg-white/25 backdrop-blur-md hover:bg-white/35 transition duration-300'
              : 'bg-white/15 backdrop-blur-md hover:bg-white/25 transition duration-300'
          ]" :style="{
            left: `${block.xAxis * 221}px`,
            top: `${block.yAxis * 195}px`,
            width: `${block.width * 221}px`,
            height: `${block.height * 195}px`,
          }">

            <div class="w-full h-[5rem] flex justify-end center-y">
              <p class="mr-5">
                < {{ block.message }} />
              </p>
            </div>

          </div>

        </div>

      </main>


    </div>
  </div>



</template>
