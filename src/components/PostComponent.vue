<template>
  <div class="container">
    <h1>Things to Do Today:</h1>
    <div class="create-posts">
      <div class="container-fluid">
        <div class="row">
          <div class="col-md-11">
            <input
              type="text"
              id="create-posts"
              v-model="text"
              placeholder="Create a Post"
            />
          </div>
          <div class="col-md-1">
            <button 
            v-on:click="createPost" 
            class="btn btn-secondary"
            >Post</button>
          </div>
        </div>
      </div>
    </div>
    <hr border-width:10/>
    <p class="error" v-if="error">{{ error }}</p>
    <div class="posts-container">
      <div
        class="post"
        v-for="(post, index) in posts"
        v-bind:item="post"
        v-bind:index="index"
        v-bind:key="post._id"
        v-on:click="deletePost(post._id)"
      >
        {{
          `${post.createdAt.getDate()}/${post.createdAt.getMonth()}/${post.createdAt.getFullYear()}`
        }}
        <p class="text">{{ post.text }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import PostService from "../PostService";
export default {
  name: "PostComponent",
  data() {
    return {
      posts: [],
      error: "",
      text: "",
    };
  },
  async created() {
    try {
      this.posts = await PostService.getPosts();
    } catch (err) {
      this.error = err.messege;
    }
  },
  methods: {
    async createPost() {
      await PostService.insertPost(this.text);
      this.posts = await PostService.getPosts();
    },
    async deletePost(id) {
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div.container {
  max-width: 800px;
  margin: 0 auto;
}

p.error {
  border: 1px solid #ff5b5f;
  background-color: #ffc5c1;
  padding: 10px;
  margin-bottom: 15px;
}

div.post {
  position: relative;
  background-color: #a2b9bc;
  padding: 0px 0px 0px 0px;
  margin-bottom: 15px;
}

div.created-at {
  position: absolute;
  top: 0;
  left: 0;
  padding: 5px 15px 5px 15px;
  background-color: rgb(0, 0, 0);
  color: white;
  font-size: 13px;
}

p.text {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 0;
}

input[type="text"] {
  width: 100%;
  padding: 12px 20px;
  margin: 0px 0;
  box-sizing: border-box;
  border: none;
  background-color: #a1a1a1;
  color: white;
}
</style>
