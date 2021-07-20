<template>
	<div class="post">
		<div class="loading" v-if="!post">
      <Spinner />
    </div>
    <div class="error" v-if="error">
      {{ error }}
    </div>
		<h3>{{ post.title }}</h3>
		<p>{{ post.body }}</p>
		<span
			class = "tag"
			v-for = "tag in post.tags"
			:key = "tag"
		>
			#{{ tag }}
		</span>
	</div>
</template>

<script>
import Spinner from '../components/Spinner.vue';
import getPost from '../composables/getPost.js';

export default {
	props: ['id'],
	components: { Spinner },
	setup(props) {

    const { post = [], error, load } = getPost(props.id);

    load();

		return {
			post,
      error
		}
	}
}
</script>

<style>
  .tags a {
    margin-right: 10px;
  }
  .post {
    max-width: 1200px;
    margin: 0 auto 4rem;
  }
  .post p {
    color: #444;
    line-height: 1.5em;
  }
  .pre {
    white-space: pre-wrap;
  }
</style>