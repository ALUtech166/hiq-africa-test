<template>
   <section class="back">
      <div class="flex justify-evenly p-8 text-center">
         <strong class="text-3xl cursor-pointer"><span class="text-yellow-500">OM</span><span class="text-white">DB.</span></strong>
         <router-link to="/bookmark" class="cursor-pointer">
            <img class="w-10" src="https://dl.dropbox.com/s/s9fb1d9vughng58/bookmark-64.png"/>
            <span class="text-white font-bold">{{ bookmarkCount }}</span>
         </router-link>
      </div>
      <div class="
         py-8 text-center mb-8 flex flex-col gap-4">
         <h1 class="font-bold text-6xl text-white">
            Discover the ultimate 
            movies
         </h1>

         <div class="flex justify-center">
  <div class="mb-3 xl:w-1/2">
    <div class="relative flex w-full flex-wrap items-stretch">
      <input
        v-model="keyWord"
        v-on:keyup.enter="getData"
        type="text" placeholder="Type  your movies title here" 
        class=" font-bold relative m-0 -mr-px block w-[1%] min-w-0 flex-auto rounded-l border border-solid border-neutral-300 bg-clip-padding px-3 py-1.5 text-base text-md text-black outline-none transition duration-300 ease-in-out focus:border-primary focus:text-black focus:shadow-te-primary focus:outline-none dark:text-black dark:placeholder:text-black"
        aria-label="Search"
        aria-describedby="button-addon1" />
      <button
        @click="getData" type="button"
        class="relative z-[2] flex items-center rounded-r bg-yellow-500 px-6 py-2.5 text-xs font-medium uppercase leading-tight text-white shadow-md transition duration-150 ease-in-out hover:bg-primary-700 hover:shadow-lg focus:bg-primary-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-primary-800 active:shadow-lg"
        id="button-addon1"
        data-te-ripple-init
        data-te-ripple-color="light">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
          fill="currentColor"
          class="h-5 w-5">
          <path
            fill-rule="evenodd"
            d="M9 3.5a5.5 5.5 0 100 11 5.5 5.5 0 000-11zM2 9a7 7 0 1112.452 4.391l3.328 3.329a.75.75 0 11-1.06 1.06l-3.329-3.328A7 7 0 012 9z"
            clip-rule="evenodd" />
        </svg>
      </button>
    </div>
  </div>
</div>
      </div>
      
      
      <EmptyState :isLoad="isLoad" :show-empty-state="showEmptyState" :is-empty-state="isEmptyState" ></EmptyState>
      
      <div v-if="response && !isLoad" class="text-center mt-12">
  <strong class="text-xl text-white font-bold">Search results for '{{ keyWordText }}'</strong>
  <div class="container p-8 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 justify-center">
    <template v-for="item in response" :key="item.imdbID">
      <Card :title="item.Title" :poster="item.Poster" :id-movie="item.imdbID" :year="item.Year" :type="item.Type"></Card>
    </template>
  </div>
</div>
      
   </section>

 

</template>

<script>
  import axios from 'axios';
  import Card from '../components/Card.vue';
  import EmptyState from '../components/EmptyState.vue';
  import API from '../apiKey.js';

  export default {
    name: 'Home',

    components: {
      Card,
      EmptyState,
    },

    data() {
      return {
        bookmarkCount: 0,
        badgeActive: 'Movie',
        keyWord: '',
        keyWordText: '',
        response: '',
        responseStatus: 'true',
        isEmptyState: true,
        showEmptyState: true,
        isLoad: false,
        errorMsg: null,
        key: API.omdb.key,
        endPoint: API.omdb.endPoint,
      };
    },

    watch: {
      responseStatus(val) {
        if (val === 'False') {
          this.isEmptyState = false;
          this.showEmptyState = true;
        } else {
          this.showEmptyState = false;
        }
      },
    },

    mounted() {
      if (localStorage.getItem('listMovie_omdb')) {
        this.response = JSON.parse(localStorage.getItem('listMovie_omdb'));
        this.responseStatus = 'True';
        this.keyWordText = localStorage.getItem('lastKeyWord_omdb');
      }

      if (localStorage.getItem('listBookmark_omdb')) {
        const bookmark = JSON.parse(localStorage.getItem('listBookmark_omdb')).bookmark;
        this.bookmarkCount = bookmark.length;
      }
    },

    methods: {
      async getData() {
        const validKeyWord = this.keyWord.split(' ').join('+');
        if (validKeyWord.split('').length > 0) {
          this.showLoader(true);

          try {
            const res = await axios.get(`${this.endPoint}?apikey=${this.key}&s=${validKeyWord}&type=${this.badgeActive}`);

            this.response = res.data.Search;
            this.responseStatus = res.data.Response;
            this.keyWordText = this.keyWord;
            this.showLoader(false);

            if (this.responseStatus === 'True') {
              localStorage.setItem('listMovie_omdb', JSON.stringify(res.data.Search));
              localStorage.setItem('lastKeyWord_omdb', this.keyWord);
            }
          } catch (err) {
            this.errorMsg = err;
          }
        }
      },

      showLoader(show) {
        if (show) {
          this.showEmptyState = true;
          this.isLoad = true;
        } else {
          this.showEmptyState = false;
          this.isLoad = false;
        }
      },

      badgeClick(name) {
        this.badgeActive = name;
        this.getData();
      },
    },
  };
</script>

<style lang="scss">
   
.back {
  background-image: url(./back.png);
  opacity: 0.9;
}
</style>
