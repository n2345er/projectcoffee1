<template>
  <div class="user-list-container">
    <h1>Get All Users</h1>
    <div>
      <!-- <button @click="navigateTo('/user/create')">สร้างผู้ใช้</button> -->
    </div>
    <hr>

    <div v-if="users.length">
      <div class="user-count">
        <b>จำนวนผู้ใช้งาน:</b> {{ users.length }}
      </div>

      <div v-for="userItem in users" :key="userItem.id" class="user-item">
        <div><b>ID:</b> {{ userItem.id }}</div>
        <div><b>ชื่อผู้ใช้:</b> {{ userItem.name }} {{ userItem.lastname }}</div>
        <div><b>อีเมล:</b> {{ userItem.email }}</div>
        <div><b>Status:</b> {{ userItem.status }}</div>
        <div><b>Type:</b> {{ userItem.type }}</div>
        <div class="user-actions">
          <button @click="navigateTo('/user/' + userItem.id)">ดูข้อมูล</button>

          <!-- แสดงปุ่มแก้ไข/ลบถ้าผู้ใช้เป็น admin -->
          <div v-if="user && user.type === 'admin'">
            <button @click="navigateTo('/user/edit/' + userItem.id)">แก้ไขข้อมูล</button>
            <button @click="deleteUser(userItem)">ลบข้อมูล</button>
          </div>
        </div>
        <hr>
      </div>
    </div>

    <!-- ปุ่ม Logout จะแสดงเมื่อมีการล็อกอิน -->
    <div v-if="user" class="logout-button">
      <!-- <button @click="logout">Logout</button> -->
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      users: [],
      user: null, // เก็บข้อมูลผู้ใช้ปัจจุบัน
    };
  },
  async created() {
    await this.refreshData(); // โหลดข้อมูลผู้ใช้
    this.user = this.$store.state.user; // ดึงข้อมูลผู้ใช้ปัจจุบันจาก store
  },
  methods: {
    async refreshData() {
      try {
        this.users = (await UsersService.index()).data;
        this.user = this.$store.state.user; // ดึงข้อมูลผู้ใช้จาก store ทุกครั้งที่ refresh
      } catch (err) {
        console.error(err);
      }
    },
    logout() {
      this.$store.dispatch("setToken", null);
      this.$store.dispatch("setUser", null);
      this.$router.push({ name: "login" });
    },
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteUser(userItem) {
      const result = confirm("คุณต้องการลบข้อมูลใช่หรือไม่?");
      if (result) {
        try {
          await UsersService.delete(userItem);
          await this.refreshData(); // refresh ข้อมูลหลังจากลบผู้ใช้สำเร็จ
        } catch (err) {
          console.error(err);
        }
      }
    },
  },
};
</script>

<style scoped>
.user-list-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.user-count {
  margin-bottom: 20px;
  font-size: 18px;
  font-weight: bold;
}

.user-item {
  margin-bottom: 20px;
}

.user-actions button {
  margin-right: 10px;
}

.logout-button {
  text-align: right;
}
</style>
