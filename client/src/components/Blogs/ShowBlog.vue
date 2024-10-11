<template>
  <div class="show-blog-container">
    <div class="show-blog-content">
      <div class="thumbnail-container">
        <img :src="BASE_URL + blog.thumbnail" alt="Blog Thumbnail" class="blog-thumbnail" />
      </div>
      <div class="blog-details">
        <h2>{{ blog.title }}</h2>
        <p class="blog-category"><strong>Category:</strong> {{ blog.category }}</p>
        <div class="blog-content" v-html="blog.content"></div>
        <div class="button-container">
          <button @click="navigateTo('/blogs')" class="back-button">Back to Home</button>
          <div v-if="isUserLoggedIn && isAdmin">
            <button @click="navigateTo('/blog/edit/' + blog.id)" class="edit-button">Edit</button>
            <button @click="deleteBlog(blog)" class="remove-button">Delete Blog</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BlogsService from '@/services/BlogsService';

export default {
  data() {
    return {
      blog: {},
      BASE_URL: 'http://localhost:8081/assets/uploads/',
    };
  },
  async created() {
    try {
      let blogId = this.$route.params.blogId;
      this.blog = (await BlogsService.show(blogId)).data;
    } catch (error) {
      console.log(error);
    }
  },
  computed: {
    isUserLoggedIn() {
      return this.$store.state.user !== null;
    },
    isAdmin() {
      return this.$store.state.userType == "admin";
    },
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteBlog(blog) {
      let result = confirm("Want to delete?");
      if (result) {
        try {
          await BlogsService.delete(blog);
          this.$router.push({ name: 'blogs' });
        } catch (err) {
          console.log(err);
        }
      }
    },
  },
};
</script>

<style scoped>
.show-blog-container {
  display: flex;
  justify-content: center;
  padding: 30px;
  background-color: #f9f9f9;
}

.show-blog-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 800px;
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.thumbnail-container {
  display: flex;
  justify-content: center;
  width: 100%;
  margin-bottom: 20px;
}

.blog-thumbnail {
  width: 100%;
  max-width: 500px;
  height: auto;
  object-fit: cover;
  border-radius: 10px;
}

.blog-details {
  display: flex;
  flex-direction: column;
  width: 100%;
}

h2 {
  font-size: 1.8rem;
  color: #333;
  margin-bottom: 10px;
  text-align: center;
}

.blog-category {
  font-size: 1rem;
  color: #888;
  margin-bottom: 15px;
  text-align: center;
}

.blog-content {
  font-size: 1rem;
  color: #555;
  line-height: 1.6;
  text-align: justify;
  margin-bottom: 20px;
}

.button-container {
  display: flex;
  justify-content: center;
  gap: 10px; /* Reduced the gap between buttons */
  margin-top: 20px;
}

.back-button, .edit-button, .remove-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.back-button {
  background-color: #f0ad4e;
  color: white;
}

.back-button:hover {
  background-color: #ec971f;
}

.edit-button {
  background-color: #5cb85c;
  color: white;
}

.edit-button:hover {
  background-color: #4cae4c;
}

.remove-button {
  background-color: #d9534f;
  color: white;
}

.remove-button:hover {
  background-color: #c9302c;
}
</style>
