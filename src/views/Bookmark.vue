<template>
   
   <section class="back text-white ">
      <Nav></Nav>
      <div v-if="isEmptyList" class="bookmark-empty">
         <strong>
            there is no item you saved here, try saving a new item
         </strong>
         <i class="far fa-sad-tear"></i>
      </div>
      <div class="container px-8 pb-8 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 justify-center">

      <template v-if="!isEmptyList" v-for="item in listBookmarks" :key="item.IdMovie">
         <div class="flex justify-center">
  <div
    class="block max-w-sm rounded-lg bg-white shadow-lg dark:bg-neutral-700">
    <a href="#!" data-te-ripple-init data-te-ripple-color="light">
      <img
        class="rounded-t-lg"
        :src="item.Poster" alt="item.Poster" />
    </a>
    <div class="p-6">
      <h5
        class="mb-2 text-xl font-medium leading-tight text-neutral-800 dark:text-neutral-50">
        <router-link :to="'/details/'+item.IdMovie" >{{ item.Title }}</router-link>
      </h5>
      <p class=" text-base text-neutral-600 dark:text-neutral-200 flex flex-col gap-4">
         <small>{{ item.Year }} | {{ item.Type }}</small>
               <i @click="deleteItem(item.IdMovie)" class="fa fa-trash text-red-500 cursor-pointer" ></i>
      </p>
      
    </div>
   </div>
  </div>

        
      </template>
   </div>
   </section>
</template>

<script>
import Nav from '../components/Back.vue'
import 'vuejs-noty/dist/vuejs-noty.css'

export default {
  name: 'Bookmark',
  components: {
    Nav
  },
  data() {
    return {
      listBookmarks: [],
      deleteTrig: true,
      isEmptyList: false
    }
  },
  mounted() {
    this.renderBookmark()
  },
  methods: {
    deleteItem(id) {
      let local = JSON.parse(localStorage.getItem('listBookmark_omdb'))
      local.bookmark = local.bookmark.filter(item => item.IdMovie !== id)
      localStorage.setItem('listBookmark_omdb', JSON.stringify(local))
      this.deleteTrig = !this.deleteTrig
      this.$noty.success('Your profile has been saved!', { layout: 'topRight' })
    },
    renderBookmark() {
      const local = localStorage.getItem('listBookmark_omdb')
      if (local) {
        this.listBookmarks = JSON.parse(local).bookmark

        if (this.listBookmarks.length === 0) {
          this.isEmptyList = true
        }
      } else {
        this.isEmptyList = true
      }
    }
  },
  watch: {
    deleteTrig() {
      setTimeout(() => {
        this.renderBookmark()
      }, 500)
    }
  }
}
</script>


<style lang="scss">
        .back {
  background-image: url(./back.png);
  opacity: 0.8;
}
   
</style>