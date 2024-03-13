<template>
  <div class="container">
    <h1 class="h1 text-center mt-3">todolist</h1>

    <input
      type="text"
      v-model.trim="text"
      @change="textChange"
      class="my-3 w-100 rounded-3 p-3 textInput border-0"
      placeholder="請輸入代辦事項"
    />
    <ul class="w-100 list-unstyled">
      <li v-for="item in textArray" :key="item.context" class="p-2 d-flex align-item-center">
        <input
          type="checkbox"
          id="item.context"
          class="form-check-input me-1"
          v-model="item.inputSelect"
        />
        <label
          :for="item.context"
          class="form-check-label align-middle"
          :class="{ checked: item.inputSelect }"
        >
          {{ item.context }}</label
        >
        <button type="button" @click="delItem(item)" class="btn btn-secondary ms-4 btn-sm">
          <i class="bi bi-trash"></i> 刪除
        </button>
      </li>
    </ul>

    <button class="btn-secondary btn ms-auto d-block mt-2" @click="allDel">
      <i class="bi bi-trash-fill"></i>
      全部清除
    </button>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, onUpdated, reactive, ref } from 'vue'
const text = ref('')

const textArray: any[] = reactive([])
function textChange() {
  textArray.push({
    context: text.value,
    inputSelect: false
  })
  text.value = ''
}

function delItem(item: object) {
  console.log(item)
  textArray.splice(textArray.indexOf(item), 1)
}

function allDel() {
  textArray.splice(0)
}

// 存入localstorage
onUpdated(() => {
  setStorage()
})
function setStorage() {
  window.localStorage.setItem('data', JSON.stringify(textArray))
}

// 讀取localstorage
onMounted(() => {
  getStorage()
})
function getStorage() {
  let data: any = window.localStorage.getItem('data')

  JSON.parse(data).forEach((item: any) => {
    textArray.push(item)
  })
}
</script>
