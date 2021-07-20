<template>
  <div class="create">
    <form @submit.prevent="addPost">
      <label>Title:</label>
      <input v-model="title" type="text" required>
      <label>Content:</label>
      <textarea v-model="body" required></textarea>
      <label>Tags (hit enter to add a tag):</label>
      <input
        @keydown.enter.prevent="handleKeydown"
        v-model="tag"
        type="text"
      >
      <div v-for="tag in tags" :key="tag" class="pill">
        #{{ tag }}
      </div>
      <button>Add Post</button>
      <p :class="{ 'active': showMessage }" class="message">
        Post added!
      </p>
    </form>
  </div>
</template>

<script>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

export default {
  setup() {
    const title = ref('');
    const body = ref('');
    const tags = ref([]);
    const tag = ref('');
    const showMessage = ref(false);
    const router = useRouter();

    // Add tags
    const handleKeydown = () => {
      if (!tags.value.includes(tag.value)) {
        tag.value = tag.value.replace(/\s/g,''); // remove all whitespace
        tags.value.push(tag.value);
      }
      tag.value = '';
    }

    // Add post to DB
    const addPost = async () => {
      try {
        const newPost = {
          title: title.value,
          body: body.value,
          tags: tags.value
        }

        await fetch('http://localhost:3000/posts', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(newPost)
        });

        showMessage.value = true;
        setTimeout(() => {
          showMessage.value = false;
          router.push({ name: 'Home' });
        }, 1000);
        title.value = '';
        body.value = '';
        tags.value = [];
      }

      catch(err) {
        alert(err);
      }
    }

    return {
      body,
      title,
      tags,
      tag,
      handleKeydown,
      showMessage,
      addPost
    }
  },
}
</script>

<style lang="scss">
  form {
    max-width: 480px;
    margin: 0 auto;
    text-align: left;
  }
  input, textarea {
    display: block;
    margin: 10px 0;
    width: 100%;
    box-sizing: border-box;
    padding: 10px;
    border: 1px solid #eee;
  }
  textarea {
    height: 160px;
  }
  label {
    display: inline-block;
    margin-top: 30px;
    position: relative;
    font-size: 20px;
    color: white;
    margin-bottom: 10px;
  }
  label::before {
    content: "";
    display: block;
    width: 100%;
    height: 100%;
    background: #ff8800;
    position: absolute;
    z-index: -1;
    padding-right: 40px;
    left: -30px;
    transform: rotateZ(-1.5deg);
  }
  button {
    display: block;
    margin-top: 30px;
    background: #ff8800;
    color: white;
    border: none;
    padding: 8px 16px;
    font-size: 18px
  }
  .pill {
    display: inline-block;
    margin: 10px 10px 0 0;
    color: #444;
    background: #ddd;
    padding: 8px;
    border-radius: 20px;
    font-size: 14px;
  }
  .message {
		margin-top: 1rem;
		text-align: center;
		padding: 10px;
		color: white;
		border-radius: 6px;
    background-color: green;
		transition: all 0.5s linear;
		opacity: 0;
		visibility: hidden;
		&.active {
			opacity: 1;
			visibility: visible;
		}
	}
</style>