<template>
  <div>
    <div class="nv-navbar">
      <ul class="nav">
        <!-- กลุ่มเมนู Blogs และ Users อยู่ซ้ายสุด -->
        <li><router-link :to="{ name: 'blogs' }">Blogs</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>

        <!-- AddItem อยู่ทางซ้ายตามหลัง Blogs และ Users -->
        <li v-if="isUserLoggedIn && isAdmin">
          <a href="#" v-on:click="navigateTo('/blog/create')">AddItem</a>
        </li>

        <!-- แสดง Logout เมื่อผู้ใช้ล็อกอิน, ถ้าไม่ล็อกอินแสดง Login -->
        <li v-if="isUserLoggedIn">
          <a href="#" v-on:click.prevent="logout">Logout</a>
        </li>
        <li v-else>
          <router-link :to="{ name: 'login' }">Login</router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      users: [],
      user: null // เก็บข้อมูลผู้ใช้ปัจจุบัน
    };
  },
  async created() {
    await this.refreshData(); // โหลดข้อมูลผู้ใช้
    this.user = this.$store.state.user; // ดึงข้อมูลผู้ใช้ปัจจุบันจาก store
  },
  computed: {
    isUserLoggedIn() {
      return this.$store.state.user !== null;
    },
    isAdmin() {
      return this.$store.state.userType == "admin";
    },
    currentUser() {
      return this.$store.state.user;
    }
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route);
    },
    async refreshData() {
      try {
        this.users = (await UsersService.index()).data;
        this.user = this.$store.state.user;
      } catch (err) {
        console.log(err);
      }
    },
    logout() {
      this.$store.dispatch('setToken', null);
      this.$store.dispatch('setUser', null);
      this.$router.push({ name: 'login' });
    },
  }
};
</script>

<style scoped>
/* Make the navbar thinner */
.nv-navbar {
  background-color: #5dade2; /* Soft blue background */
  width: 100%;
  padding: 15px 0; /* Thinner padding */
  border-radius: 10px; /* Slight rounded corners */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Slight shadow for depth */
  margin-bottom: 20px; /* Add some space below the navbar */
}

.nv-navbar .nav {
  display: flex;
  justify-content: flex-start; /* Align items to the left */
  align-items: center;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: 15px; /* Reduced gap between items */
}

.nv-navbar .nav li a {
  padding: 8px 15px; /* Smaller padding for a thinner appearance */
  text-decoration: none;
  color: white; /* White text */
  font-weight: bold;
  font-size: 1rem;
  transition: background-color 0.3s ease, color 0.3s ease;
  border-radius: 6px; /* Slightly smaller rounded buttons */
}

.nv-navbar .nav li a:hover {
  background-color: #2e86c1; /* Darker blue on hover */
  color: #f1f1f1;
}

.nv-navbar .nav li a.router-link-active {
  background-color: #2874a6; /* Active link background */
  color: white;
}

/* Align Logout/Login button to the right */
.nv-navbar .nav li:last-child {
  margin-left: auto;
}

body {
  background-color: #ebf5fb; /* Light background */
  font-family: 'Arial', sans-serif;
}
</style>
