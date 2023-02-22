<template>
  
   <section v-if="!isWaiting" class="back text-white">
   <Nav></Nav>
   <Waiting :is-waiting="isWaiting"></Waiting>
   <div class="justify-center p-8">
  <div
    class="flex flex-col rounded-lg bg-white shadow-lg dark:bg-neutral-700 container">
    <img
      class="h-full w-full rounded-t-lg object-cover md:h-full md:w-48 md:rounded-none md:rounded-l-lg"
      :src="movie.Poster" :alt="movie.Title" />
    <div class="flex flex-col justify-start p-6">
      <h5
        class="mb-2 text-xl font-medium text-neutral-800 dark:text-neutral-50">
        <strong>{{ movie.Title }}</strong>
      </h5>
      <p class="mb-4 text-md text-black justify-center">
         <p>{{ movie.Year }} • {{ movie.Genre }} • {{ movie.Runtime }}</p>
      </p>
      <p class="text-sm text-black">
         <Rating :rate="parseFloat(movie.Rating)"></Rating>
      </p>
      <div class="text-md text-black justify-center w-2/3 pb-4">
         <p>Plot</p>
         <p>
            {{ movie.Plot }}
         </p>
         <p>Actors & Production by</p>
         <p>
            {{ movie.Actors }} • {{ movie.Production }}
         </p>
      </div>
      <Button :details-movie="movie"></Button>
    </div>
  </div>
</div>
      
   </section>
</template>

<script>
   
   import axios from 'axios'
   import Nav from '../components/Back.vue'
   import Rating from '../components/Rating.vue'
   import Button from '../components/Button.vue'
   import Waiting from '../components/Waiting.vue'
   import API from '../apiKey.js'
   
   export default {
      name: 'Details',
      components: {
         Nav,
         Rating,
         Button,
         Waiting
      },
      data() {
         return {
            errResponse: false,
            movie: {},
            endPoint: API.omdb.endPoint,
            key: API.omdb.key,
            isWaiting: true,
         }
      },
      mounted() {
         let idMovie = this.$route.params.idMovie
         let url = `${this.endPoint}?apikey=${this.key}&i=${idMovie}`
         
         axios.get(url)
            .then( res => {
               
               let movie = this.movie
               let info = res.data
               movie.IdMovie = info.imdbID
               movie.Poster = info.Poster
               movie.Title = info.Title
               movie.Year = info.Year
               movie.Genre = info.Genre
               movie.Runtime = info.Runtime
               movie.Rating = info.imdbRating
               movie.Plot = info.Plot
               movie.Production = info.Production
               movie.Actors = info.Actors
               movie.Type = info.Type
               
               this.isWaiting = false
            })
            .catch( err => console.log(err))
      }
   }
   
</script>

<style lang="scss">
   .back {
  background-image: url(./back.png);
  opacity: 0.8;
}
   
</style>