<template lang="pug">
div(class="px-20 pb-20 pt-5")
  div(class="mb-2 text-2xl text-slate-200 font-bold text-left") Nested Key-Value Pair Tree Viewer
  div(class="grid grid-cols-2 gap-8 h-full")
    div(class="border border-slate-500")
      div(class="flex justify-end p-3")
        button(class="bg-slate-200 text-slate-800 p-2 font-bold") ＋Add New Pair
      div
        div(class="px-5 py-2 flex")
          div(class="grid grid-cols-2 gap-4 flex-grow mr-3")
            input(class="bg-transparent outline-none border text-slate-200 border-slate-500 text-base px-2 py-1 w-full" placeholder="Key")
            input(class="bg-transparent outline-none border text-slate-200 border-slate-500 text-base px-2 py-1 w-full" placeholder="Value")
          button(class="w-10 text-sm flex justify-center items-center bg-slate-300 text-slate-800") －
    div(class="text-slate-200 p-6 border border-slate-500")
      div(class="text-lg") 
        div(class="flex cursor-pointer") 
          span(class="font-bold") nav
          span(class="ml-1") [-]
        div(class="pl-8") 
          div(class="text-lg") 
            div(class="flex cursor-pointer") 
              span(class="font-bold") header
              span(class="ml-1") [-]
            div(class="pl-8") 
              div(class="text-lg") 
                div(class="inline") 
                  span(class="font-bold") creator
                div(class="inline") 
                  span(class="ml-1") :
                  span(class="ml-3 text-rose-300") 3D Fabric Creator
          div(class="text-lg") 
            div(class="inline") 
                span(class="font-bold") product
            div(class="inline") 
              span(class="ml-1") :
              span(class="ml-3 text-rose-300") Productname
          div(class="text-lg") 
              div(class="inline") 
                span(class="font-bold") icon
              div(class="inline") 
                span(class="ml-1") :
                span(class="ml-3 text-rose-300") Icon name
</template>

<script setup>

</script>
