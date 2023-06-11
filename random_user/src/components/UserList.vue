<template lang="pug">
div(class="flex cursor-pointer mt-5")
    div(class="relative")
        img(:src="thisUser?.picture.large" @click="dialog = true")
        img(
            @click="onSetLike"
            :src="isLike ? fillHeart : emptyHeart"
            class="absolute bottom-1 right-2 w-6 h-6"
            )
    div(class="ml-3 text-left")
        div Name: {{ thisUser.name.title + ". " + thisUser.name.first + " " + thisUser.name.last }}
        div Gender: {{ thisUser.gender }}
        div Location: {{ thisUser.location.country }}
        div Email: {{ thisUser.email }}
        div Phone: {{ thisUser.phone }}
div(
  @click="dialog = false"
  :class="dialog === false ? 'hidden' : 'fixed'"
  class="inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-full w-full z-10 flex justify-center items-center"
  )
  user-model(
    v-model:show="dialog"
    :class="dialog === false ? 'hidden' : 'block z-20'"
    :data="thisUser"
    )
</template>

<script setup>
import { ref, watch } from "vue";
import UserModel from "./UserModel.vue";
import emptyHeart from "../assets/favorite_empty.svg";
import fillHeart from "../assets/favorite_fill.svg";
import { set, del, get } from 'idb-keyval';

const props = defineProps(["data", "isLikeMode", "showMode"])
const thisUser = ref(props.data);

// 是否已加入我的最愛
const isLike = ref(false);
// 從 indexdb 取出加入最愛的清單
const init = async () => {
    await get(props.data.cell).then((val) => {
        isLike.value = val !== undefined
    });
}

watch(props, () => {
    if (props.isLikeMode) {
        if (typeof props.data === "string") {
            thisUser.value = JSON.parse(props.data);
        }
        else thisUser.value = props.data;
    }
    else {
        thisUser.value = props.data;
    }
    init();
}, { immediate: true })

// 彈出視窗狀態
const dialog = ref(false);

// 加入我的最愛
const onSetLike = async () => {
    isLike.value = !isLike.value
    if (isLike.value) {
        await set(props.data.cell, JSON.stringify(props.data))
        .then(() => console.log('set 儲存成功'))
        .catch((err) => console.log('set 儲存失敗', err));
    }
    else{
        await del(props.data.cell)
        .then(() => console.log('del 刪除成功'));
    }

}
</script>
