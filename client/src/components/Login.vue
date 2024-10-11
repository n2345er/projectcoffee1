<template>
  <div class="login-container">
    <div class="login-box">
      <h1>User Login</h1>
      <form v-on:submit.prevent="onLogin" class="login-form">
        <div class="form-group">
          <label for="email">Email</label>
          <input type="email" id="email" name="email" v-model="email" required />
        </div>
        <div class="form-group">
          <label for="password">Password</label>
          <input
            type="password"
            id="password"
            name="password"
            v-model="password"
            required
          />
        </div>
        <div class="button-group">
          <button class="login-button" type="submit">Login</button>
          <button class="register-button" v-on:click="navigateTo('/user/create')">Create Account</button>
        </div>
        <div class="error-message" v-if="error">{{ error }}</div>
      </form>
    </div>
  </div>
</template>

<script>
import AuthenService from "../services/AuthenService";

export default {
  data() {
    return {
      email: "",
      password: "",
      error: null,
    };
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route);
    },
    async onLogin() {
      try {
        const response = await AuthenService.login({
          email: this.email,
          password: this.password,
        });

        this.$store.dispatch("setToken", response.data.token);
        this.$store.dispatch("setUser", response.data.user);
        
        const userType = response.data.user.type;
        this.$store.dispatch("setUserType", userType);

        if (userType === 'admin') {
          this.$router.push({ name: "admin" });
        } else {
          this.$router.push({ name: "users" });
        }
      } catch (error) {
        console.log(error);
        this.error = error.response.data.error;
        this.email = "";
        this.password = "";
      }
    },
  },
};
</script>

<style scoped>
/* Main container for the login page */
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f7f7f7;
}

/* Box that contains the login form */
.login-box {
  background-color: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  text-align: center;
  width: 100%;
  max-width: 400px;
}

/* Form styling */
.login-form {
  display: flex;
  flex-direction: column;
  text-align: left;
}

.form-group {
  margin-bottom: 20px;
  text-align: left; /* Aligns text to the left */
}

.form-group label {
  font-weight: bold;
  color: #555;
  margin-bottom: 8px;
  display: block; /* Ensures labels take up the full width */
}

.form-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
  box-sizing: border-box;
  transition: border-color 0.3s ease;
}

.form-group input:focus {
  border-color: #5cb85c; /* Green border on focus */
  outline: none;
}

/* Button styling */
.button-group {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.login-button, .register-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.login-button {
  background-color: #5cb85c;
  color: white;
}

.login-button:hover {
  background-color: #4cae4c;
}

.register-button {
  background-color: #f0ad4e;
  color: white;
}

.register-button:hover {
  background-color: #ec971f;
}

/* Error message styling */
.error-message {
  color: red;
  margin-top: 20px;
  font-weight: bold;
}
</style>
