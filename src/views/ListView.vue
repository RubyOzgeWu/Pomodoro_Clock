<template lang="pug">
v-row#list
  v-col(cols='12')
    h1.my-3(style="color: #546E7A;") 我的任務
  v-col(cols='12')
    v-text-field(
      placeholder="新增一個任務"
      variant="underlined"
      v-model="newItem"
      append-icon="mdi-plus"
      :rules="[required]"
      @click:append="onInputSubmit"
      @keydown.enter='onInputSubmit'
    )
    v-table
      thead
        tr
          th(style="font-size: 24px;") 任務清單
          th
      tbody
        tr(v-if="items.length === 0")
          td.text-center(colspan="2" style="font-size: 22px;") 沒有資料
        tr(v-for="(item, i) in items")
          td(style="font-size: 22px;")
            v-text-field(v-if="item.edit" v-model="item.model" autofocus)
            span(v-else) {{ item.name }}
          td
            span(v-if="item.edit")
              v-btn(icon variant="plain" flat color="light-green accent-1" @click="confirmEditItem(i)")
                v-icon mdi-check
              v-btn(icon variant="plain" flat color="red" @click="cancelEditItem(i)")
                v-icon mdi-undo
            span(v-else)
              v-btn(icon variant="plain" flat color="green darken-4" @click="editItem(i)")
                v-icon mdi-pencil
              v-btn(icon variant="plain" flat color="red" @click="delItem(i)")
                v-icon mdi-delete
</template>

<style>
.v-table{
  background: #99aab2;
  border-radius: 10px;
}
#input-5{
  font-size: 24px;
}

</style>

<script setup>
import { ref } from 'vue'
import { storeToRefs } from 'pinia'
import { useListStore } from '@/stores/list'

const newItem = ref('')
const required = value => {
  return !!value
}

const list = useListStore()
const { items } = storeToRefs(list)
const { addItem, delItem, editItem, confirmEditItem, cancelEditItem } = list

const onInputSubmit = () => {
  const valid = required(newItem.value)
  if (!valid) return
  addItem(newItem.value)
  newItem.value = ''
}
</script>
