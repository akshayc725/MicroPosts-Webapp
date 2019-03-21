<template>
  <div class="container">
    <h1>Latest Posts</h1>
    <div class="create-post">
      <label for="create-post">What you have to show??</label>
      <input type="text" id="create-post" v-model="text" placeholder="Create a post">
      <button v-on:click="createPost">Post!</button>
    </div>
    <hr>
    <p class="error" v-if="error">{{ error }}</p>
    <div class="post-container">
      <div class="post"
      v-for="(post, index) in posts"
      v-bind:item="post"
      v-bind:index="index"
      v-bind:key="post._id"
      v-on:dblclick="deletePost(post._id)"
      >
       {{ `${post.createdAt.getDate()}/
           ${post.createdAt.getMonth()}/
           ${post.createdAt.getFullYear()}`
       }}
       <p class="text">{{ post.text }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import postService from '../postService';

export default {
  name: 'PostComponent',
  data(){
    return{
      posts: [],
      error: '',
      text: ''
    }
  },
  async created() {
    try{
      this.posts = await postService.getPosts(); 
    }catch(err){
      this.error = err.message;
    }
  },
  methods: {
    async createPost() {
      await postService.insertPost(this.text);
      this.posts = await postService.getPosts(); 
    },
    async deletePost(id) {
      await postService.deletePost(id);
      this.posts = await postService.getPosts(); 
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div.container{
  max-width: 800px;
  margin: 0 auto;
}

p.error{
  border: 1px solid #ff5b5f;
  background-color: #ffc5c1;
  padding: 10px;
  margin-bottom: 15px;
}

div.post{
  position: relative;
  border: 1px solid #5bd658;
  background-color: #bcffb8;
  padding: 10px 10px 30px 10px;
  margin-bottom: 15px;
}

div.create-at{
  position: absolute;
  top: 0px;
  left: 0px;
  padding: 5px 15px 5px 15px;
  background-color: darkgreen;
  color: white;
  font-size: 13px;
}

p.text{
  font-size: 13px;
  font-weight: 700;
  margin-bottom: 0;
}
</style>
