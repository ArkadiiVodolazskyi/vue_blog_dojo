<template>
	<h2>All posts matching tag: {{ tag }}</h2>
	<div class="layout">
		<PostsList :posts="postsWithTag" />
		<TagsCloud :posts="posts" />
	</div>
	<div class="loading" v-if="!postsWithTag.length">
		<Spinner />
	</div>
	<div class="error" v-if="error">
		{{ error }}
	</div>
</template>

<script>
import PostsList from '../components/PostsList.vue';
import Spinner from '../components/Spinner.vue';
import TagsCloud from '../components/TagsCloud.vue';
import getPosts from '../composables/getPosts.js';
import { useRoute } from 'vue-router';
import { computed } from 'vue';

export default {
	components: { PostsList, Spinner, TagsCloud },
	setup() {
		const route = useRoute();
    const { posts, error, load } = getPosts();

    load();

		const tag = route.params.tag;

		const postsWithTag = computed(() => {
			return posts.value.filter(post => {
				return post.tags.includes(tag)
			});
		});

    return {
      postsWithTag,
      error,
			posts,
			tag
    }
  }
}
</script>

