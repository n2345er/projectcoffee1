<template>
  <div class="create-user-container">
    <h1>Create User</h1>
    <form @submit.prevent="createUser" class="user-form">
      <div class="form-field">
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="user.name" />
      </div>
      <div class="form-field">
        <label for="lastname">Lastname:</label>
        <input type="text" id="lastname" v-model="user.lastname" />
      </div>
      <div class="form-field">
        <label for="email">Email:</label>
        <input type="text" id="email" v-model="user.email" />
      </div>
      <div class="form-field">
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="user.password" />
      </div>
      <div class="form-field">
        <button type="submit" class="btn-submit">Create User</button>
      </div>
    </form>
  </div>
</template>

<script>
import UsersService from '../../services/UsersService';

export default {
  data() {
    return {
      user: {
        name: '',
        lastname: '',
        email: '',
        password: '',
        type: 'user',
        status: 'active',
      },
    };
  },
  methods: {
    async createUser() {
      try {
        await UsersService.post(this.user);
        this.$router.push('/users');
      } catch (err) {
        console.error(err);
      }
    },
  },
};
</script>

<style scoped>
.create-user-container {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f7f7f7;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

.user-form .form-field {
  margin-bottom: 15px;
}

.user-form label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.user-form input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.btn-submit {
  width: 100%;
  padding: 10px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn-submit:hover {
  background-color: #218838;
}
</style>
