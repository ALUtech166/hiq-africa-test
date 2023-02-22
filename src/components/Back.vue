<template>
   <section>
      <div @click="$router.go(-1)" class="cursor-pointer p-4">
         <i class="fa fa-chevron-left" ></i>
         <strong>Back</strong>
      </div>
      <strong class="nav-title">
         {{ navName }}
      </strong>
   </section>
</template>

<script>
   
   export default {
      name: 'Back',
      inheritAttrs: false,
      props: {
         navName: {
            type: String
         }
      }
   }
   
</script>

<style lang="scss">
  
</style>