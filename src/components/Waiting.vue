<template>
   <section v-if="isWaiting" class="waiting">
      <div class="waiting-box">
         <strong>Please wait, fetching data from server</strong>
         <i class="fa fa-smile-beam" ></i>
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