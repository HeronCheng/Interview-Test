<template lang="pug">
div(class="px-20 pb-20 pt-5")
  div(class="mb-2 text-2xl text-slate-200 font-bold text-left") Nested Key-Value Pair Tree Viewer
  div(class="grid grid-cols-2 gap-8 h-full")
    div(class="border border-slate-500")
      div(class="flex justify-end p-3")
        button(@click="onAddNew" class="bg-slate-200 text-slate-800 p-2 font-bold") ＋Add New Pair
      div
        div(
          v-for="(item, index) in list"
          :key="item.key"
          class="px-5 py-2 flex"
          )
          div(class="grid grid-cols-2 gap-4 flex-grow mr-3")
            input(
              @change="onEditKey(item)"
              :value="item.key"
              :id="'key' + index"
              placeholder="Key"
              class="bg-transparent outline-none border text-slate-200 border-slate-500 text-base px-2 py-1 w-full"
              )
            input(
              @change="onEditValue(item)"
              :value="item.value"
              :id="'value' + index"
              class="bg-transparent outline-none border text-slate-200 border-slate-500 text-base px-2 py-1 w-full"
              placeholder="Value"
              )
          button(@click="onDelete(item)" class="w-10 text-sm flex justify-center items-center bg-slate-300 text-slate-800") －
    div(class="text-slate-200 p-6 border border-slate-500")
      div(id="rightPart" class="text-lg text-left") 
</template>

<script setup>
import { ref, watch } from "vue"

const list = ref([])

// 新增一條紀錄
function onAddNew() {
  list.value.push({
    key: "",
    value: "",
  })
}
// 刪除一條紀錄
function onDelete(item) {
  const findDeleteItem = (element) => element.key === item.key;
  const delIndex = list.value.findIndex(findDeleteItem);
  list.value.splice(delIndex, 1);
}
// 修改key
function onEditKey(item) {
  const findEditItem = (element) => element.key === item.key;
  const editIndex = list.value.findIndex(findEditItem);
  list.value[editIndex].key = document.getElementById("key" + editIndex).value;
}
// 修改value
function onEditValue(item) {
  const findEditItem = (element) => element.value === item.value;
  const editIndex = list.value.findIndex(findEditItem);
  list.value[editIndex].value = document.getElementById("value" + editIndex).value;
  handleTreeViewer();
}

// 處理右方顯示區的邏輯
const objectTree = ref({});
const handleTreeViewer = () => {
  const newArr = [...list.value];

  const result = {};

  newArr.forEach(item => {
    const keys = item.key.split('.');
    const lastKey = keys.pop();

    let currentObj = result;
    keys.forEach(key => {
      if (!currentObj[key]) {
        currentObj[key] = {};
      }
      currentObj = currentObj[key];
    });

    currentObj[lastKey] = item.value;
  });
  objectTree.value = result;

  const container = document.getElementById('rightPart');
  if (container?.innerHTML) container.innerHTML = '';
  // 進入 generateHTML 這個函式的次數
  let count = 0;
  generateHTML(objectTree.value, container);

  function generateHTML(obj, parentElement) {
    count++;
    const keys = Object.keys(obj);

    keys.forEach(key => {
      const value = obj[key];
      const element = document.createElement('div');
      if (count > 1) {
        element.classList.add('pl-8');
      }
      // 顯示key和value
      const keyElement = document.createElement('span');
      keyElement.classList.add('cursor-pointer');
      if (typeof value === 'object') {
        keyElement.innerText = key;
        element.appendChild(keyElement);
        const accordion = document.createElement('span');
        accordion.innerText = '[-]';
        accordion.classList.add('ml-1');
        element.appendChild(accordion);
      }
      else {
        keyElement.innerText = key + ' : ';
        element.appendChild(keyElement);
      }

      if (typeof value === 'object') {
        generateHTML(value, element);
      } else {
        const valueElement = document.createElement('span');
        valueElement.classList.add('text-rose-300');
        valueElement.innerText = value;
        element.appendChild(valueElement);
      }

      parentElement.appendChild(element);
    });
  }
}

watch(list, handleTreeViewer, { immediate: true });
</script>
