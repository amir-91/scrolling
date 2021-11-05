<template>
<div>
    <div class="container" v-for="post in posts" :key="post.id">
          <h3>Name: {{post.name}}</h3>
          <h5>email: {{post.email}}</h5>
          <p>{{post.body}}</p>
    </div>
       <div ref="infinitescrolltrigger" id="scoll-trigger"></div>
       <div class="loader" v-if="loading"></div>
    </div>
</template>




<script>
import axios from 'axios'

export default {
    data () {
      return {
           posts: [],
           limit: 10,
           loading: false,
           lastDataId: 0,
           lastPostId: 0,
      }
  },

    methods: {
    loadMore() {
      if(this.lastElementId === this.lastPostId) {
          return
        }
      this.loading = true;
      axios.get("https://jsonplaceholder.typicode.com/comments").then(response => {
        this.lastElementId = response.data[response.data.length-1].id
        console.log(this.lastElementId)
        const append = response.data.slice(
          this.posts.length,
          this.posts.length + this.limit
        );
        this.posts = this.posts.concat(append);
        this.lastPostId = this.posts[this.posts.length-1].id
        console.log(this.lastPostId)
        this.loading = false;
      });
    },
     callback(entries) {
      entries.forEach(entry => {
    if (entry.isIntersecting) {
       this.loadMore();
    }
  });
},
},
     created() {
       this.loadMore();
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
.loader {
  border: 16px solid #f3f3f3; /* Light grey */
  border-top: 16px solid #3498db; /* Blue */
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 2s linear infinite;
  margin: auto;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>