<template>
<div class="container mx-auto">
  <div class="max-w-sm rounded-lg bg-white shadow-lg dark:bg-neutral-700">
    <a href="#!" data-te-ripple-init data-te-ripple-color="light">
      <img class="rounded-md" :src="poster" alt="poster" />
    </a>
    <div class="p-6">
      <h5 class="mb-2 text-xl font-medium leading-tight text-neutral-800 dark:text-neutral-50">
        <router-link :to="'/details/' + idMovie">
          <h3>{{ title }}</h3>
        </router-link>
      </h5>
      <p class="mb-4 text-base text-neutral-600 dark:text-neutral-200">
        <small>{{ year + " | " + type }}</small>
      </p>
    </div>
  </div>
</div>

  
</template>

<script>
  export default {
    name: 'Card',
    props: {
      title: {
        type: String,
        default: 'Iron Man 3'
      },
      year: {
        type: String,
        default: '2013'
      },
      type: {
        type: String,
        default: 'Movie'
      },
      idMovie: {
        type: String,
        required: true
      },
      poster: {
        type: String,
        default: 'src/assets/poster.jpeg'
      }
    }
  }
</script>

<style lang="scss" >
   
   
</style>