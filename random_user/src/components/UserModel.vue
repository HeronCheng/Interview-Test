<template lang="pug">
div(class="flex bg-white rounded p-5 shadow-lg")
    img(:src="data.picture.large")
    div(class="ml-3 text-left")
        div Name: {{ data.name.title + ". " + data.name.first + " " + data.name.last }}
        div Gender: {{ data.gender }}
        div Location: {{ data.location.country }}
        div Email: {{ data.email }}
        div Phone: {{ data.phone }}
</template>

<script setup>
defineProps(["show", "data"]);
const emit = defineEmits(["update:show"]);

</script>
