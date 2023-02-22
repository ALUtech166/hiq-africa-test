<template>
   <section class="button-bookmark">
      <div class="flex gap-4">
         <button 
         :disabled="isBookmark"
         @click="pushToBookmark" 
         class="inline-block rounded bg-black px-6 pt-2.5 pb-2 text-xs font-medium uppercase leading-normal text- shadow-[0_4px_9px_-4px_#e4a11b] transition duration-150 ease-in-out hover:bg-black-500 hover:shadow-[0_8px_9px_-4px_rgba(228,161,27,0.3),0_4px_18px_0_rgba(228,161,27,0.2)] focus:bg-warning-600 focus:shadow-[0_8px_9px_-4px_rgba(228,161,27,0.3),0_4px_18px_0_rgba(228,161,27,0.2)] focus:outline-none focus:ring-0 active:bg-warning-700 active:shadow-[0_8px_9px_-4px_rgba(228,161,27,0.3),0_4px_18px_0_rgba(228,161,27,0.2)]" type="button">
            <template v-if="isBookmark" >
               Saved
            </template>
            <template v-else >
               Bookmark
            </template>
         </button>
         <button
         @click="sharePage" 
         type="button"
         class="inline-block rounded bg-yellow-500 px-6 pt-2.5 pb-2 text-xs font-medium uppercase leading-normal text- shadow-[0_4px_9px_-4px_#e4a11b] transition duration-150 ease-in-out hover:bg-yellow-500 hover:shadow-[0_8px_9px_-4px_rgba(228,161,27,0.3),0_4px_18px_0_rgba(228,161,27,0.2)] focus:bg-warning-600 focus:shadow-[0_8px_9px_-4px_rgba(228,161,27,0.3),0_4px_18px_0_rgba(228,161,27,0.2)] focus:outline-none focus:ring-0 active:bg-warning-700 active:shadow-[0_8px_9px_-4px_rgba(228,161,27,0.3),0_4px_18px_0_rgba(228,161,27,0.2)]">
          Share <i class="fa fa-share-alt" ></i>
         </button>
      </div>
   </section>
   <pre>
      {{ test }}
   </pre>
</template>

<script>
   
   export default {
      name: 'Button',
      data() {
         return {
            movie: {},
            test: '',
            isBookmark: false,
         }
      },
      inheritAttrs: false,
      props: {
         detailsMovie: {
            type: Object
         }
      },
      methods: {
         pushToBookmark() {
            let local = localStorage.getItem('listBookmark_omdb')
            let movie = this.movie
            let details = this.detailsMovie
            
            movie.Poster = details.Poster
            movie.Title = details.Title,
            movie.Year = details.Year,
            movie.Type = details.Type
            movie.IdMovie = details.IdMovie
             
            if (local) {
               local = JSON.parse(local);
               
               //console.log(local, movie)
               //console.log(this.isDuplicate(local, movie))
               
               if ( this.isDuplicate(local, movie) === false) {
                  local.bookmark.push(movie)
                  localStorage.setItem('listBookmark_omdb', JSON.stringify(local))
               }
               
            } else {
               local = { bookmark: []}
               local.bookmark.push(movie)
               localStorage.setItem('listBookmark_omdb', JSON.stringify(local))
            }
            
            this.isBookmark = true
            
         },
         isDuplicate(local, movie) {
            let duplicate = false;
            for ( const item of local.bookmark) {
               if ( item.IdMovie == movie.IdMovie ) duplicate = true
            }
            return duplicate
         },
         sharePage() {
            if (navigator.share) {
               navigator.share({
                 title: `OMDB Movie Details | ${this.detailsMovie.Title}`,
                 url: ''
               }).then(result => {
                 console.log(result)
               }).catch(err => {
                 console.error(err)
               })
            }
         }
      },
      mounted() {
         console.log('mounted')
         let local = JSON.parse(localStorage.getItem('listBookmark_omdb'))
         let id = this.$route.params.idMovie
         
         if (local) {
            for ( const item of local.bookmark ) {
               if ( item.IdMovie == id ) this.isBookmark = true
            }
         }
         
      }
   }
   
</script>

<style lang="scss">
   
</style>