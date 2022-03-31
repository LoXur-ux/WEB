<template>
  <div :class="[checkWidth() ? 'container-pc' : 'container-phone']">
    <img v-if="isPC" src="../assets/left.png" class="left-img" />
    <img v-else src="../assets/top.png" class="top-img" />
    <div class="main-form" aria-hidden="false">
      <p>Login</p>
      <label for="login-inp" class="input-form">
        <input id="login-inp" type="text" />
      </label>

      <p>Password</p>
      <div class="pass-block input-form">
        <label for="pass-inp">
          <input id="pass-inp" :type="passType" />
        </label>
        <img class="show-pass" src="../assets/opened-eye.svg" @mousedown="switchEye" />
      </div>
      <button class="login" @click="sendRequest">Login!</button>
    </div>
  </div>
</template>

<script lang="ts">
import axios from "axios";

export default {
  name: "LoginForm",
  data: () => ({
    isPC: Boolean,
    isShowPass: Boolean,
    passType: String,
    url: String,
  }),

  created() {
    this.isPC = this.checkWidth();
    this.isShowPass = true;
    this.switchEye();
    this.url = "https://ca4952d7-b451-4173-96a4-879f680514d1.mock.pstmn.io";
  },

  methods: {
    checkWidth(): boolean {
      return window.innerWidth > 640;
    },

    switchEye() {
      this.isShowPass = !this.isShowPass;
      if (this.isShowPass) this.passType = "text";
      else this.passType = "password";
    },

    sendRequest() {
      const url = this.url + "/auth/check";
      const headers = {
        "Content-Type": "application/json",
        "x-mock-match-request-body": "true",
      };

      const data = {
        login: (document.getElementById("login-inp") as HTMLInputElement).value,
        password: (document.getElementById("pass-inp") as HTMLInputElement).value,
      };

      axios
        .post(url, data, { headers })
        .then((res) => {
          alert("Successful Authorization!");
        })
        .catch((err) => {
          alert("Somthing was wrong!");
        });
    },
  },
};
</script>

<style scoped>
.container-pc {
  width: 100%;
  margin: 0 0;
  display: flex;
  justify-content: center;
}

.container-phone {
  width: 100%;
  display: block;
}

.left-img {
  width: 30%;
}

.top-img {
  width: 100%;
}

.main-form {
  min-width: 360px;
  max-width: 640px;

  margin-top: 36px;
  text-align: center;
}

p {
  margin: 4px 0;
}
.pass-block {
  display: flex;
  justify-content: center;
  margin-left: -0.5em;
}

input {
  text-align: center;
  font-size: 1.5em;
  border: 3px solid transparent;
  border-radius: 0px;
  border-image: linear-gradient(45deg, #6e5642, #253547);
  -moz-border-image: -moz-linear-gradient(45deg, #6e5642, #253547);
  -webkit-border-image: -webkit-linear-gradient(45deg, #6e5642, #253547);
  border-image-slice: 1;
  margin-bottom: 16px;
}

.show-pass {
  height: 1.5em;
  vertical-align: center;
  margin-left: -2em;
  margin-top: 4px;
}

.login {
  width: 280px;
  height: 48px;
  margin: 16px 0;
  font-size: 2em;
  color: white;

  background: linear-gradient(45deg, #6e5642, #253547);
  border: 0;
  border-radius: 6px;
  border-color: #e7d379;
}

.login:hover {
  transform: scale(1.05);
}

.login:active {
  transform: scale(0.98);
}
</style>
