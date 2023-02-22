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
    };
  },
  inheritAttrs: false,
  props: {
    detailsMovie: {
      type: Object,
      required: true,
    },
  },
  methods: {
    async pushToBookmark() {
      const local = JSON.parse(localStorage.getItem('listBookmark_omdb')) || {
        bookmark: [],
      };
      const movie = {
        Poster: this.detailsMovie.Poster,
        Title: this.detailsMovie.Title,
        Year: this.detailsMovie.Year,
        Type: this.detailsMovie.Type,
        IdMovie: this.detailsMovie.IdMovie,
      };

      if (!this.isDuplicate(local, movie)) {
        local.bookmark.push(movie);
        localStorage.setItem('listBookmark_omdb', JSON.stringify(local));
      }

      this.isBookmark = true;
    },

    isDuplicate(local, movie) {
      return local.bookmark.some(item => item.IdMovie === movie.IdMovie);
    },

    async sharePage() {
      if (navigator.share) {
        try {
          await navigator.share({
            title: `OMDB Movie Details | ${this.detailsMovie.Title}`,
            url: '',
          });
        } catch (err) {
          console.error(err);
        }
      }
    },
  },
  async mounted() {
    console.log('mounted');
    const local = JSON.parse(localStorage.getItem('listBookmark_omdb')) || {
      bookmark: [],
    };
    const id = this.$route.params.idMovie;

    this.isBookmark = local.bookmark.some(item => item.IdMovie === id);
  },
};
</script>


<style lang="scss">
   
</style>