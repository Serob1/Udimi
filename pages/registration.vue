<template>
  <div class="general_page">
    <div class="login_section">
      <h1>Create Account</h1>
      <div class="form_section">
        <div>
          <label>Name*</label>
          <input placeholder="Name" type="text" v-model="name" />
          <p class="error" v-if="errors.name">{{ errors.name }}</p>
        </div>
        <div>
          <label>Email*</label>
          <input placeholder="Email" type="email" v-model="email" />
          <p class="error" v-if="errors.email">{{ errors.email }}</p>
        </div>
        <div>
          <label>Password*</label>
          <input placeholder="Password" type="password" v-model="password" />
          <p class="error" v-if="errors.password">{{ errors.password }}</p>
        </div>
      </div>
      <button class="login_btn" @click="handleClick" :disabled="loading">
        Registration
      </button>
      <div class="already_have_an_account">
        <span>
          <NuxtLink to="/login">Already have an account?</NuxtLink>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Registration",
  data() {
    return {
      name: "",
      email: "",
      password: "",
      errors: {
        name: "",
        email: "",
        password: "",
      },
      loading: false,
    };
  },
  methods: {
    handleClick() {
      this.loading = true;
      let self = this;
      axios
        .post("https://api.quwi.com/v2/auth/signup", {
          name: this.name,
          email: this.email,
          password: this.password,
        })
        .then(function (response) {
          self.loading = false;
          if (response.status === 200) {
            self.$router.push("/");
          }
        })
        .catch(function (error) {
          self.loading = false;
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
  justify-content: center;
  align-items: center;
    height: 100vh;
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
        margin-top: 20px;
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
    .already_have_an_account {
      margin-top: 40px;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
}
</style>
