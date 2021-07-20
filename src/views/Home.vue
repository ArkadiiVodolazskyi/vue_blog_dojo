<template>
  <div class="home">
    <h2>All posts</h2>
    <div class="layout">
      <PostsList :posts="posts" />
      <TagsCloud :posts="posts" />
    </div>
    <div class="loading" v-if="!posts.length">
      <Spinner />
    </div>
    <div class="error" v-if="error">
      {{ error }}
    </div>
  </div>
</template>

<script>
import PostsList from '../components/PostsList.vue';
import Spinner from '../components/Spinner.vue';
import TagsCloud from '../components/TagsCloud.vue';
import getPosts from '../composables/getPosts.js';

export default {
  components: { PostsList, Spinner, TagsCloud },
  name: "Home",
  setup() {
    const { posts, error, load } = getPosts();

    load();

    return {
      posts,
      error
    }
  }
};
</script>

<style>
  .home {
    max-width: 1200px;
    margin: 0 auto;
    padding: 10px;
  }
  .layout {
    display: grid;
    grid-template-columns: 3fr 1fr;
    gap: 20px;
  }
</style>