<template>
  <div>
    <div v-if="login">
      <home :acc_present="acc_present" :acc="acc" />
    </div>
    <div :class="{ login: login }">
      <div class="top"></div>
      <div class="main">
        <div class="alert alert-success alert-dismissible" v-if="success">
          <button type="button" class="close" data-dismiss="alert">
            &times;
          </button>
          Đăng Nhập Thành Công
        </div>
        <div class="alert alert-danger alert-dismissible" v-if="fail">
          <button type="button" class="close" data-dismiss="alert">
            &times;
          </button>
          Sai tên đăng nhập hoặc mật khẩu
        </div>
        <div class="alert alert-warning alert-dismissible" v-if="warn">
          <button type="button" class="close" data-dismiss="alert">
            &times;
          </button>
          Vui lòng nhập đầy đủ thông tin
        </div>
        <input
          type="text"
          placeholder="Tài Khoản"
          v-model="name"
          @keyup="nowarn()"
        />
        <input
          type="password"
          placeholder="Mật Khẩu"
          v-model="pass"
          @keyup="nowarn()"
        />
        <button class="submit" @click="submit()">Đăng Nhập</button>
      </div>
      <div class="bot"></div>
    </div>
  </div>
</template>
<script>
import Home from "./Home";
export default {
  name: "Login",
  components: { Home },
  methods: {
    submit() {
      if (!this.pass || !this.name) {
        this.warn = true;
      } else {
        const account = this.acc.filter(
          (element) =>
            element.username === this.name && element.pass === this.pass
        );
        if (account.length !== 0) {
          this.success = true;
          this.fail = false;
          this.acc_present = account[0];
          setTimeout(() => {
            this.login = true;
          }, 2000);
        } else {
          console.log("Sai thông tin đăng nhập");
          this.success = false;
          this.fail = true;
        }
      }
    },
    nowarn() {
      this.warn = false;
    },
  },
  props: { acc: Array },
  computed: {},
  data() {
    return {
      name: "",
      pass: "",
      success: false,
      fail: false,
      warn: false,
      login: false,
      acc_present: "",
    };
  },
};
</script>

<style scoped>
.login {
  display: none;
}
.top {
  height: 120px;
  width: 100%;
  background-color: #fddb3a;
  position: relative;
  margin-top: -60px;
}
.bot {
  height: 100px;
  width: 100%;
  background-color: #fddb3a;
  margin-top: 520px;
}
.main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 90%;
  height: 540px;
  position: absolute;
  background-color: #e5e5e5;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border-radius: 17px;
  margin: -13px 5%;
}
input {
  width: 385px;
  height: 55px;
  border-radius: 10px;
  border: 1px solid #e0d7d7;
  padding-left: 20px;
  margin-bottom: 10px;
}
.forget-pass {
  margin-left: 142px;
  margin-top: 0px;
  color: #5a5757;
  font-family: DM Sans;
  font-style: normal;
  font-weight: 500;
  font-size: 13px;
  line-height: 20px;
}
.submit {
  width: 405px;
  height: 55px;
  padding: 10px;
  background: #fddb3a;
  box-shadow: 0px 1px 2px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
}
</style>
