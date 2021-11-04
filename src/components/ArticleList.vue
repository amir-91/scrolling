<template>
<div>
    <div class="container" v-for="article in articles" :key="article.id">
          <h3>Name: {{article.name}}</h3>
          <h5>email: {{article.email}}</h5>
          <p>{{article.body}}</p>
    </div>
       <div ref="infinitescrolltrigger" id="scoll-trigger"></div>
    </div>
</template>




<script>
import axios from 'axios'

export default {
    data () {
     return  {
         articles: [],
         observer: null,
     }
    },
    computed: {
      pageCount() {
        return Math.ceil(this.totalResults/this.maxPerPage);
      },
      pageOffset() {
        return this.maxPerPage * this.currentPage;
      }
    },
    methods: {
      callback(entries) {
      entries.forEach(entry => {
    if (entry.isIntersecting) {
       this.fetchUsers();
    }
  });
},
       fetchUsers() {
         for (var i = 0; i < 5; i++) {
        axios.get(`https://jsonplaceholder.typicode.com/comments`).then(response => {
          this.articles.push(response.data[i]);
          this.showloader = false;
        });
      }
      },
    },
    mounted() {
      this.observer = new IntersectionObserver(this.callback);
      this.observer.observe(this.$refs.infinitescrolltrigger);
}
}
</script>


<style scoped>
.container {
      width: 500px;
      margin: auto;
      padding: 10px;
      border: 2px solid grey;
      box-shadow: grey;
      border-radius: 10px;
      text-align: left;
      margin-bottom: 10px;
   }
</style>