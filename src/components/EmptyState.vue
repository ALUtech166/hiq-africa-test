<template>
   <div v-if="showEmptyState" class="state-wrapper">
      <div v-if="isEmptyState && !isLoad" class="content">
         <strong>Search results will be displayed here</strong>
         <router-link to="/bookmark" class="button-to-bookmark" >
            To bookmark
         </router-link>
      </div>
      <div v-if="!isEmptyState && !isLoad" class="content">
         <strong>Sorry, movie not found</strong>
         <div class="button-to-bookmark" >
            <i class="far fa-sad-tear"></i>
         </div>
      </div>
      <div v-if="isLoad" class="content">
         <strong>Please Wait</strong>
         <div class="button-to-bookmark">
            <i class="far fa-smile-beam"></i>
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
            default: true
         },
         showEmptyState: {
            type: Boolean,
            default: true
         },
         isLoad: {
            type: Boolean,
            default: false
         }
      }
   }
   
</script>

<style lang="scss" >
   
  
   
</style>