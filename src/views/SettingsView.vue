<template lang="pug">
v-row#settings
  v-col(cols="12")
    h1.my-3.text-center(style="color: #546E7A;") 選擇鈴聲
  v-col(cols="12")
    v-table
      thead
        tr
          th(style="font-size: 20px;") 名稱
          th(style="font-size: 20px;") 試聽
          th(style="font-size: 20px;") 選擇
      tbody
        tr(v-for="alarm in alarms")
          td(style="font-size: 22px;") {{ alarm.name }}
          td
            audio(controls :src="alarm.file")
          td
            v-radio(v-model="selectedAlarm" :value="alarm.id")
</template>

<style>
.v-table{
  background: #99aab2;
  border-radius: 10px;
}

</style>

<script setup>
import { useSettingsStore } from '@/stores/settings'
import { storeToRefs } from 'pinia'

const settings = useSettingsStore()
const { selectedAlarm, alarms } = storeToRefs(settings)
</script>
