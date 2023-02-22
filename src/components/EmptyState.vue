<template>
   <div v-if="showEmptyState" class="flex flex-col items-center justify-center">
     <div v-if="isEmptyState && !isLoad" class="text-center">
       <p class="font-medium text-lg mb-3">Search results will be displayed here</p>
       <router-link to="/bookmark" class="bg-blue-500 hover:bg-blue-600 text-white font-medium py-2 px-4 rounded">
         To bookmark
       </router-link>
     </div>
     <div v-if="!isEmptyState && !isLoad" class="text-center">
       <p class="font-medium text-white text-lg mb-3">Sorry, movie not found</p>
       <div class="bg-white rounded-full p-3">
         <i class="far fa-sad-tear text-gray-600 text-3xl"></i>
       </div>
     </div>
     <div v-if="isLoad" class="text-center">
       <p class="font-medium text-white text-lg mb-3">Please Wait</p>
       <div class="bg-white rounded-full p-3">
         <i class="far fa-smile-beam text-yellow-500 text-3xl animate-spin"></i>
       </div>
     </div>
   </div>
 </template>
 <script>
 export default {
   name: 'EmptyState',
   props: {
     isEmptyState: {
       type: Boolean,
       default: true,
     },
     showEmptyState: {
       type: Boolean,
       default: true,
     },
     isLoad: {
       type: Boolean,
       default: false,
     },
   },
 };
 </script>
 <style lang="scss" >
   // no custom styles needed
 </style> 