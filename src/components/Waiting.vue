<template>
   <section v-if="isWaiting" class="flex justify-center items-center h-screen">
     <div class="bg-white rounded-lg p-8">
       <h2 class="text-lg font-bold mb-4">Please wait, fetching data from server</h2>
       <i class="far fa-smile-beam text-5xl text-indigo-500"></i>
     </div>
   </section>
 </template>
 
<script>
   
   export default {
      name: 'Waiting',
      props: {
         isWaiting: {
            type: Boolean,
            default: true
         }
      }
   }
   
</script>

<style lang="scss">
   
</style>