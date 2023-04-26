<template>
  <div class="container">
    <h1>Latest Posts</h1>
    <div class="create-post">
      <label for="create-post">Post Content: </label>
      <input type="text" id="create-post" v-on:keyup.enter="createPost" v-model="text" placeholder="What's new?">
      <button v-on:click="createPost">POST!</button>
    </div>
    <hr>
    <p class="error" v-if="error">{{ error }}</p>
    <div class="post-container">
      <div class="post"
        v-for="(post, index) in posts"
        v-bind:item="post"
        v-bind:index="index"
        v-bind:key="post._id"
      >
        {{ `${post.createdAt.getDate()}/${post.createdAt.getMonth()}/${post.createdAt.getFullYear()}` }}
        <p class="text">{{ post.text }}</p>
        <button v-on:click="deletePost(post._id)">x</button>
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
      error: '',
      text: '',
    };
  },
  async created() {
    try {
      this.posts = await PostService.getPosts();
    } catch (err) {
      this.error = err.message;
    }
  },
  methods: {
    async createPost() {
      if (this.text !== ""){
        await PostService.insertPost(this.text);
        this.text = "";
      }
      this.posts = await PostService.getPosts();
    },
    async deletePost(id) {
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  color: #42B883;
}

div.container {
  max-width: 800px;
  margin: auto;
  
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

input {
  height: 1.5rem;
  padding-left: 1rem;
  color: #42B883;
  background: #1A1A20;
  border-top: 0;
  border-bottom: 1px solid #42B883;
  border-left: 0;
  border-right: 0;
  border-radius: 0;
}

input:focus {
  border-radius: 0.5rem;
  outline: 1px solid #42B883;
}

button {
  height: 1.8rem;
  color: #42B883;
  background: #1A1A20;
  border: 2px solid #42B883;
  border-radius: 0.1rem;
  margin-left: 0.2rem;
}

button:active {
  height: 1.8rem;
  color: #1A1A20;
  background: #42B883;
  border: 2px solid #42B883;
  border-radius: 0.1rem;
}

hr {
  position: relative;
  width: 300%;
  left: -100%;
  margin-top: 2rem;
  border: 1px solid #42B883;
}

p.error {
  border: 1px solid #ff5b5f;
  background-color: #ffc5c1;
  padding:  10px;
  margin-bottom: 15px;
}

div.create-post {
  color: #42B883;
  position: relative;
  width: 100%;
}

div.post-container {
  width: 100%;
  color: #42B883;
}

div.post {
  margin-top: 2rem;
}

div.post button {
  position: relative;
  height: 1.5rem;
  top: 0;
  right: 0;
}

div.created-at {
  position: absolute;
  top: 0;
  left: 0;
  padding: 5px 15px 5px 15px;
  background-color: darkgreen;
  color: white;
  font-size: 13px;
}

p.text {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 0;
}
</style>
