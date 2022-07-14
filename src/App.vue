<template lang="pug">
v-app
  v-app-bar(color="teal darken-1")
    v-app-bar-title(style="font-weight: 500; color: white; font-size: 30px;") Pomodoro Clock
    v-spacer
    v-btn(icon to='/' color="white")
      v-icon mdi-timer
    v-btn(icon to='/list' color="white")
      v-icon mdi-format-list-bulleted
    v-btn(icon to='/settings' color="white")
      v-icon mdi-cog
    v-btn(icon v-if="notify" color="white" @click="toggleNotify" )
      v-icon mdi-bell
    v-btn(icon v-else color="white" @click="toggleNotify")
      v-icon mdi-bell-off
  v-main
    v-container
      router-view(v-slot="{ Component }")
        keep-alive(include="HomeView")
          component(:is="Component")
</template>

<script setup>
import { storeToRefs } from 'pinia'
import { useListStore } from '@/stores/list'
const list = useListStore()
const { notify } = storeToRefs(list)
const { toggleNotify } = list
</script>
