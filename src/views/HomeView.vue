<template lang="pug">
v-row.text-center(width="100%")#home
  v-card.my-5.timer(outlined max-width="700")
    v-col.my-3(cols="12")
      h1.my-3(style="font-size: 48px; color: #78909C") {{ currentText }}
      h1.my-3(style="font-size: 48px; color: #546E7A;") {{ timeText }}
    v-col.btn.my-3(cols="12")
      v-btn.mx-3(v-if="status !== 1" color="green lighten-1" fab @click="startTimer" ) Play
        v-icon mdi-play
      v-btn.mx-3(v-else color="primary" fab @click="pauseTimer") Pause
        v-icon mdi-pause
      v-btn.mx-3(v-if="current.length > 0" color="error" fab @click="finishTimer(true)") Next
        v-icon mdi-skip-next

  v-col(cols="12")
    v-card.missions.my-3(v-for="(item, i) in items" outlined max-width="50%" style="margin: auto; height: 100px;")
      v-list-item(style="height: 100px;")
        v-list-item-content(style="width: 100%; display: flex; justify-content: space-around; padding: 0 15px;")
          div.text-center.t1 任務{{i+1}}
          div.text-center.t2  {{ item.name }}

</template>

<style>
.v-main__wrap{
  background-color: #B2DFDB;
}

/* timer */
.v-card.timer{
  margin: auto;
  border-radius: 10px;
  background: #E0F2F1;
}
.v-btn.mx-3{
  width: 180px;
  height: 80px;
  font-size: 30px;

}
.btn{
  display: flex;
  justify-content: center;
}

/* show */
.v-card.missions{
  background: #f1f8ebda;
  color: #797878;
}
.t1{
  width: 20%;
  font-size: 25px;
  font-weight: 500;
  /* background: lightblue; */
}
.t2{
  width: 80%;
  font-size: 25px;
  font-weight: 500;
  /* background: lightyellow; */
}

</style>

<script setup>
import { computed, ref } from 'vue'
import { storeToRefs } from 'pinia'
import { useListStore } from '@/stores/list'
import { useSettingsStore } from '@/stores/settings'

const list = useListStore()
const { current, items, timeleft } = storeToRefs(list)
const { countdown, start, finish } = list

const settings = useSettingsStore()
const { selectedAlarmFile } = storeToRefs(settings)

const currentText = computed(() => {
  return current.value.length > 0 ? current.value : items.value.length > 0 ? '點擊開始' : '沒有事項'
})
const timeText = computed(() => {
  const m = Math.floor(timeleft.value / 60).toString().padStart(2, '0')
  const s = (timeleft.value % 60).toString().padStart(2, '0')
  return m + ':' + s
})

let timer
// 0 = 停止
// 1 = 倒數中
// 2 = 暫停
const status = ref(0)

const pauseTimer = () => {
  status.value = 2
  clearInterval(timer)
}

const finishTimer = (skip) => {
  clearInterval(timer)
  status.value = 0
  finish()
  if (!skip) {
    const audio = new Audio()
    audio.src = selectedAlarmFile.value
    audio.play()
  }
  if (items.value.length > 0) {
    startTimer()
  }
}

const startTimer = () => {
  if (status.value === 0 && items.value.length > 0) {
    start()
  }
  if (current.value.length > 0) {
    status.value = 1
    timer = setInterval(() => {
      countdown()
      if (timeleft.value === 0) {
        finishTimer(false)
      }
    }, 1000)
  }
}

</script>
