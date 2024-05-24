<script setup>
import HelloWorld from './components/HelloWorld.vue'
import Hello from "./components/Hello.vue"
import DispStatus from "./components/DispStatus.vue"
import Battle from "./components/Battle.vue"
import { ref, reactive } from 'vue';

const state = reactive({
  page: "top"
})

const player = reactive({
  name: 'プレイヤー',
  mhp: 10,
  atk: 2,
  def: 1,
  spd: 1,
})

const enemy = reactive({
  name: 'エネミー',
  mhp: 10,
  atk: 2,
  def: 1,
  spd: 3,
})

const chgPage = (page_name) => {
  state.page = page_name
}
</script>

<template>
  <h1>RPG戦闘機能</h1>
  <button @click="chgPage('playerStatus')">ステータス確認</button>
  <button @click="chgPage('enemyStatus')">敵ステータス確認</button>
  <button @click="chgPage('battle')">戦闘開始</button>

  <DispStatus v-if="state.page==='playerStatus'" :character="player" />
  <DispStatus v-if="state.page==='enemyStatus'" :character="enemy" />
  <Battle v-if="state.page==='battle'" :player="player" :enemy="enemy" />
</template>

<style scoped>
</style>
