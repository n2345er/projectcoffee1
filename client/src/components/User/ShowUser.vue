<template>
  <div class="user-detail-container">
    <h1>Show User</h1>

    <div class="user-detail">
      <p><b>ID:</b> {{ user.id }}</p>
      <p><b>Name:</b> {{ user.name }}</p>
      <p><b>Lastname:</b> {{ user.lastname }}</p>
      <p><b>Email:</b> {{ user.email }}</p>
      <p><b>Password:</b> {{ user.password }}</p>
      <p><b>Status:</b> {{ user.status }}</p>
      <p><b>Type:</b> {{ user.type }}</p>
      <p><b>Created At:</b> {{ new Date(user.createdAt).toLocaleString() }}</p>
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      user: {},
    };
  },
  async created() {
    try {
      const userId = this.$route.params.userId;
      this.user = (await UsersService.show(userId)).data;
    } catch (err) {
      console.error(err);
    }
  },
};
</script>

<style scoped>
.user-detail-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

.user-detail p {
  font-size: 16px;
  line-height: 1.5;
  margin-bottom: 10px;
}

.user-detail p b {
  color: #555;
}
</style>
