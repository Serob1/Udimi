<template>
  <div class="general_page">
    <div class="login_section">
      <h1>Login Your Account</h1>
      <div class="form_section">
        <div>
          <label>Email</label>
          <input placeholder="Email" type="email" v-model="email" />
          <p class="error" v-if="errors.email">{{ errors.email }}</p>
        </div>
        <div class="pass_form">
          <label>Password</label>
          <input placeholder="Password" type="password" v-model="password" />
          <p class="error" v-if="errors.password">{{ errors.password }}</p>
          <span>Forgot password?</span>
        </div>
      </div>
      <button class="login_btn" @click="handleClick" :disabled="loading">Login</button>
      <div class="dont_have_an_account">
        <span>
          <NuxtLink to="/registration">Don't have an account?</NuxtLink>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
      errors: {
        email: "",
        password: "",
      },
      loading: false
    };
  },
  methods: {
    handleClick() {
      this.loading = true
      let self = this;
          axios
            .post("https://api.quwi.com/v2/auth/login", {
              email: this.email,
              password: this.password,
            })
            .then(function (response) {
              self.loading = false
              if (response.status === 200) {
                localStorage.token = response.data.token;
                self.$router.push("/profile");
              }
            })
            .catch(function (error) {
              self.loading = false
              self.errors = error.response.data.first_errors;
            });
    },
  },
  mounted() {
    if (localStorage.token) {
      this.$router.push("/profile");
    }
  },
};
</script>
<style scoped lang="scss">
.general_page {
  display: flex;
  height: 100vh;
  justify-content: center;
  align-items: center;
       background: linear-gradient(90deg, #3f51b5, transparent) #2196f3;
  .login_section {
    background: white;
    margin-top: 100px;
    border: 1px solid #2196f3;
    border-radius: 5%;
    padding: 100px 40px;
    h1 {
      color: #2196f3;
      font-size: 40px;
    }
    .form_section {
      .error {
        color: red;
      }
      div {
        display: flex;
        flex-direction: column;
        label {
          font-size: 20px;
        }
        input {
          outline: none;
          border: 1px solid #3f51b5;
          border-radius: 5px;
          text-indent: 5px;
          width: 350px;
          height: 40px;
        }
      }
      .pass_form {
        margin-top: 20px;
        span {
          margin-top: 5px;

          cursor: pointer;
          &:hover {
            opacity: 0.9;
          }
        }
      }
    }
    .login_btn {
      margin-top: 40px;
      transition: 0.5s;
      width: 100%;
      height: 40px;
      cursor: pointer;
      border: 1px solid #3f51b5;
      border-radius: 5px;
      color: white;
      font-size: 20px;
      background: linear-gradient(90deg, #3f51b5, transparent) #2196f3;
      transition: background-color 0.5s;
      &:hover {
        background-color: #e91e63;
      }
      &:disabled {
        opacity: 0.1;
      }
    }
    .dont_have_an_account {
      margin-top: 40px;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
}
</style>
