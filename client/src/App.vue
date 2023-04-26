<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <PostComponent />
</template>

<script>
import PostComponent from "./components/PostComponent.vue";

export default {
  name: "App",
  components: {
    PostComponent,
  },
};
</script>

<style>
img {
  width: 50%;
  margin: 25%;
}

body {
  background: #1A1A20;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
