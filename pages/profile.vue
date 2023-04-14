<template>
  <div class="profile_page">
    <h1>Welcome to your account!</h1>
    <button @click="handleLogout()">Logout</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Profile",
  methods: {
    handleLogout() {
      let self = this;
      axios
        .post("https://api.quwi.com/v2/auth/logout", {
          anywhere: true,
        })
        .then(function (response) {
          if (response.status === 200) {
            localStorage.removeItem("token");
            self.$router.push("/");
          }
        })
        .catch(function (error) {
          console.log("err", error);
        });
    },
  },
  mounted() {
    if (!localStorage.token) {
      this.$router.push("/");
    }
  },
};
</script>
<style scoped lang="scss">
.profile_page{
  height: 100vh;
  text-align: center;
  background: linear-gradient(90deg, #3f51b5, transparent) #2196f3;
  font-family: italic;
  h1{
    font-size: 50px;
    color: white;
  }
  button{
          margin-top: 40px;
      transition: 0.5s;
      width: 20%;
      height: 40px;
      cursor: pointer;
      border: 1px solid #3f51b5;
      border-radius: 5px;
      color: white;
      font-size: 20px;
      background: linear-gradient(90deg, #e91e63, transparent) #2196f3;
      transition: background-color 0.5s;
      &:hover {
        opacity: 0.8;
      }
  }
}
</style>
