<template>
  <div class="tag-cloud">
    <h3>Tags</h3>
    <router-link
      class="tag pill"
      v-for="tag in tags"
      :key="tag"
      :to="{ name: 'Tag', params: { tag: tag } }"
    >
      #{{ tag }}
    </router-link>
  </div>
</template>

<script>
import useTags from '../composables/useTags'

export default {
  setup(props) {
    const { tags } = useTags(props.posts)
    return { tags }
  },
  props: ['posts']
};
</script>

<style>
  .tag-cloud {
    padding: 10px;
  }
  .tag-cloud h3 {
    border-bottom: 1px solid #eee;
    padding: 16px 8px;
    color: #444;
  }
  .tag-cloud a {
    color: #fff;
    text-decoration: none;
    background-color: #ff8800;
  }
  .tag-cloud a.router-link-active {
    color: blue;
    font-weight: bold;
  }
</style>