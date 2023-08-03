<script setup lang="ts">
import { computed, ref } from 'vue'

type Glyph = { lvl: number; xpNeeded: number }
type Dungeon = { lvl: number; enemiesLvl: number; xp: number }

const minDungeonLvl = 54
const lvl = ref<string | undefined>()

const nightmareDungeons: Dungeon[] = [...Array(100)].map((_, idx) => ({
  lvl: idx + 1,
  enemiesLvl: minDungeonLvl + idx,
  xp: (idx + 1) * 2 + 2
}))

const glyphs: Glyph[] = [
  { lvl: 1, xpNeeded: 0 },
  { lvl: 2, xpNeeded: 8 },
  { lvl: 3, xpNeeded: 15 },
  { lvl: 4, xpNeeded: 24 },
  { lvl: 5, xpNeeded: 35 },
  { lvl: 6, xpNeeded: 46 },
  { lvl: 7, xpNeeded: 59 },
  { lvl: 8, xpNeeded: 72 },
  { lvl: 9, xpNeeded: 87 },
  { lvl: 10, xpNeeded: 104 },
  { lvl: 11, xpNeeded: 121 },
  { lvl: 12, xpNeeded: 140 },
  { lvl: 13, xpNeeded: 159 },
  { lvl: 14, xpNeeded: 180 },
  { lvl: 15, xpNeeded: 203 },
  { lvl: 16, xpNeeded: 226 },
  { lvl: 17, xpNeeded: 251 },
  { lvl: 18, xpNeeded: 276 },
  { lvl: 19, xpNeeded: 303 },
  { lvl: 20, xpNeeded: 332 },
  { lvl: 21, xpNeeded: 361 }
]

function isValidLvl(lvl: string | undefined): lvl is string {
  return !!lvl && Number.isInteger(+lvl) && +lvl <= 100
}

const displayedDungeons = computed(() =>
  isValidLvl(lvl.value)
    ? [...nightmareDungeons].splice(Math.max(+lvl.value - minDungeonLvl, 0), 11)
    : nightmareDungeons
)

function calculateTotalXpNeeded(targetGlyph: Glyph) {
  let count = 0

  for (const glyph of glyphs.values()) {
    count += glyph.xpNeeded
    if (glyph === targetGlyph) {
      return count
    }
  }
  return count
}
</script>

<template>
  <div class="max-w-screen-xl mx-auto">
    <div
      class="mb-12 border border-slate-700 p-4 rounded text-center lg:p-8 lg:max-w-screen-md lg:mx-auto"
    >
      <h2 class="text-xl mb-4">Personalize</h2>
      <div class="flex items-center justify-center space-x-2">
        <label class="font-extrabold">Your lvl:</label>
        <input
          v-model="lvl"
          type="text"
          class="max-w-[80px] peer relative block w-full flex-1 rounded bg-slate-500 outline-none transition-all duration-200 ease-linear motion-reduce:transition-none min-h-[38px] px-4"
        />
      </div>
    </div>
    <div class="flex flex-col space-y-12 justify-stretch lg:flex-row lg:space-x-12 lg:space-y-0">
      <div class="grow">
        <h2 class="text-xl mb-4">
          Nightmare Dungeons<span v-if="isValidLvl(lvl)" class="text-xs"> (adapted to your lvl)</span>
        </h2>
        <table class="rounded min-w-full border-opacity-10 text-gray-400">
          <thead class="bg-slate-800">
            <tr>
              <th>Level</th>
              <th>Max enemies lvl</th>
              <th>Glyph XP rewards</th>
            </tr>
          </thead>
          <tbody class="text-center">
            <tr
              v-for="dungeon of displayedDungeons"
              :key="dungeon.lvl"
              class="even:bg-slate-900 hover:bg-slate-700"
            >
              <td>{{ dungeon.lvl }}</td>
              <td>{{ dungeon.enemiesLvl }}</td>
              <td>{{ dungeon.xp }}</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="grow">
        <h2 class="text-xl mb-4">Glyphs XP</h2>
        <table class="rounded min-w-full border-opacity-10 text-gray-400">
          <thead class="bg-slate-800">
            <tr>
              <th>Level</th>
              <th>XP needed</th>
              <th>Total XP needed</th>
            </tr>
          </thead>
          <tbody class="text-center">
            <tr
              v-for="glyph of glyphs"
              :key="glyph.lvl"
              class="even:bg-slate-900 hover:bg-slate-700"
            >
              <td>{{ glyph.lvl }}</td>
              <td>{{ glyph.xpNeeded }}</td>
              <td>{{ calculateTotalXpNeeded(glyph) }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
