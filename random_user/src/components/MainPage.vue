<template lang="pug">
div
  div(class="flex mb-3 justify-between")
    div(class="flex")
      div(
        @click="isLikeMode = false; onShowLike()"
        class="border-b-2 mr-2 cursor-pointer"
        :class="!isLikeMode ? 'border-violet-500 text-violet-500' : 'border-violet-300 text-violet-300'"
        ) ALL
      div(
        @click="isLikeMode = true; onShowLike()"
        class="border-b-2 cursor-pointer"
        :class="isLikeMode ? 'border-violet-500 text-violet-500' : 'border-violet-300 text-violet-300'"
        ) Favorite
    div(class="flex items-center")
      div
        select(v-model="showItems" class="border rounded p-1")
          option(value = 10) 10
          option(value = 30) 30
          option(value = 50) 50
      div(
        @click="showMode = 'card'"
        class="w-5 h-5 mx-3 cursor-pointer"
        :class="showMode === 'card' ? 'bg-rose-300' : 'bg-gray-300'"
        )
      div(
        @click="showMode = 'list'"
        class="w-5 h-5 cursor-pointer"
        :class="showMode === 'list' ? 'bg-rose-300' : 'bg-gray-300'"
        )
  div(v-show="showMode === 'card'" class="grid grid-cols-5 gap-5")
    user-card(
      v-for="data in pageList"
      :key="data"
      :data="data"
      :isLikeMode="isLikeMode"
      :showMode="showMode"
    )
  div(v-show="showMode === 'list'")
    user-list(
      v-for="data in pageList"
      :key="data"
      :data="data"
      :isLikeMode="isLikeMode"
      :showMode="showMode"
    )
  
  nav(class="mt-5")
    ul(class="inline-flex -space-x-px")
      li
        a(@click="currentPage = currentPage - 1" class="cursor-pointer px-3 py-2 ml-0 mr-1.5 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700") &lt;
      div(v-if="currentPage <= 5" class="inline-flex -space-x-px")
        li
          a(@click="currentPage = 1" :class="currentPage === 1 ? 'selected' : 'nonSelected'") 1
        li
          a(@click="currentPage = 2" :class="currentPage === 2 ? 'selected' : 'nonSelected'") 2
        li
          a(@click="currentPage = 3" :class="currentPage === 3 ? 'selected' : 'nonSelected'") 3
        li
          a(@click="currentPage = 4" :class="currentPage === 4 ? 'selected' : 'nonSelected'") 4
        li
          a(@click="currentPage = 5" :class="currentPage === 5 ? 'selected' : 'nonSelected'") 5
      div(v-else class="inline-flex -space-x-px")
        li
          a(@click="currentPage = currentPage - 2" class="nonSelected") {{ currentPage - 2}}
        li
          a(@click="currentPage = currentPage - 1" class="nonSelected") {{ currentPage - 1 }}
        li
          a(@click="currentPage = currentPage" class="selected") {{ currentPage }}
        li
          a(@click="currentPage = currentPage + 1" href="#" class="nonSelected") {{ currentPage + 1 }}
        li
          a(@click="currentPage = currentPage + 2" href="#" class="nonSelected") {{ currentPage + 2}}
      li
        a(@click="currentPage = currentPage + 1" class="cursor-pointer px-3 py-2 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700") >
</template>

<script setup>
import { ref, watch } from 'vue';
import axios from 'axios';
import UserCard from './UserCard.vue';
import UserList from './UserList.vue';
import { values } from 'idb-keyval';

// 全部清單
const userList = ref([]);
// 現在顯示的清單
const pageList = ref([]);
const currentPage = ref(1);

const init = async () => {
  await axios('https://randomuser.me/api/?results=3010')
    .then((response) => {
      userList.value = response.data.results;
    })
  getCurrPage();
}
init();

// 切換 card & list 頁面
const showMode = ref("card");
// 切換全部 & 收藏模式
const isLikeMode = ref(false);
// 顯示個數
const showItems = ref(30);
// 處理頁面顯示的函式
const getCurrPage = () => {
  pageList.value = userList.value.slice((currentPage.value - 1) * Number(showItems.value), currentPage.value  * Number(showItems.value))
}
// 處理 全部 & 收藏模式顯示的函式
const onShowLike = async () => {
  if (isLikeMode.value) {
    await values().then((values) => pageList.value = values);
  }
  else {
    await init();
  }
}

watch(showItems, () => {
  getCurrPage();
})
watch(currentPage, () => {
  getCurrPage();
})
</script>

<style scoped>
.nonSelected {
  @apply cursor-pointer px-3 py-2 mr-1.5 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700
}
.selected {
  @apply cursor-pointer px-3 py-2 mr-1.5 text-blue-600 border border-gray-300 bg-blue-50 hover:bg-blue-100 hover:text-blue-700
}
</style>