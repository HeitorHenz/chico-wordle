<template>
 <div class="flex items-center justify-center h-16 col-span-1 uppercase transition-all duration-300 transform border-2 border-gray-200"
 :class="{
     'border-gray-500 bg-gray-500 text-white' :color == 'gray',
     'border-green-600 bg-green-600 text-white' :color == 'green',
     'border-yellow-500 bg-yellow-500 text-white' :color == 'yellow',

 }">
     {{letter}}
 </div>
</template>

<script setup>
const props = defineProps({
    letter:{
        type: String,
        default: ''
    },
    color:{
        type: String,
        default:''
    }
})
</script>

<style>

</style>