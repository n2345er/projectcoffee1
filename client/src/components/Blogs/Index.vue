<template>
  <div class="blog-list">
    <div v-for="blog in blogs" :key="blog.id" class="blog-item">
      <img :src="BASE_URL + blog.thumbnail" alt="Blog Thumbnail" class="thumbnail" />
      <div class="blog-info">
        <h3>{{ blog.title }}</h3>
        <p class="blog-content" v-html="blog.content"></p>
        <p class="blog-category"><strong>Category:</strong> {{ blog.category }}</p>
        <button class="details-button" @click="navigateTo(`/blog/${blog.id}`)">Read More</button>
      </div>
    </div>
  </div>
</template>

<script>
import BlogsService from '@/services/BlogsService'

export default {
  data() {
    return {
      blogs: [],
      BASE_URL: "http://localhost:8081/assets/uploads/",
    };
  },
  async created() {
    try {
      this.blogs = (await BlogsService.index()).data;
    } catch (error) {
      console.error(error);
    }
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route);
    },
  },
};
</script>

<style scoped>
.blog-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  gap: 20px;
  padding: 20px;
  background-color: #f9f9f9;
}

.blog-item {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 320px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.blog-item:hover {
  transform: translateY(-5px);
}

.thumbnail {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-bottom: 1px solid #ddd;
}

.blog-info {
  padding: 15px;
  text-align: left;
}

h3 {
  font-size: 1.5rem;
  color: #333;
  margin-bottom: 10px;
}

.blog-content {
  font-size: 1rem;
  color: #666;
  max-height: 4.5em;
  overflow: hidden;
  text-overflow: ellipsis;
}

.blog-category {
  font-size: 0.9rem;
  color: #888;
  margin-bottom: 15px;
}

.details-button {
  display: inline-block;
  background-color: #007bff;
  color: white;
  padding: 8px 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.details-button:hover {
  background-color: #0056b3;
}
</style>
