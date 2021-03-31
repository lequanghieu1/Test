<template>
  <div>
    <h1>Đây là trang chủ</h1>
    <b-button variant="primary" class="mb-5" v-b-modal="'my-modal'"
      >Thông tin tài khoản</b-button
    >
    <b-button variant="success" class="mr-2 ml-2 mb-5" v-b-modal="'my-modal1'"
      >Đăng ký tài khoản</b-button
    >
    <b-button
      variant="danger"
      v-b-modal="'my-modal2'"
      class="mb-5"
      v-if="acc_present.role === 'User'"
      >Xóa tài khoản</b-button
    >
    <Table :acc="acc" v-if="acc_present.role === 'Admin'" />
    <b-modal id="my-modal" title="Thông tin cơ bản">
      <div class="info">
        <p>Tài Khoản : {{ acc_present.username }}</p>
        <p>Tên : {{ acc_present.fullname }}</p>
        <p>Tuổi : {{ acc_present.age }}</p>
        <p>Email : {{ acc_present.email }}</p>
      </div>
    </b-modal>
    <b-modal id="my-modal1" title="Thêm Tài Khoản" @ok="handleOk">
      <b-form @submit.stop.prevent>
        <label for="feedback-user">User Name</label>
        <b-form-input
          v-model="userName"
          :state="validationU"
          id="feedback-user"
        ></b-form-input>
        <b-form-invalid-feedback :state="validationU">
          Your username must be 5-12 characters long.
        </b-form-invalid-feedback>
        <b-form-valid-feedback :state="validationU">
          Looks Good.
        </b-form-valid-feedback>
        <label for="feedback-user">Pass Word</label>
        <b-form-input
          v-model="password"
          :state="validationP"
          id="feedback-user"
        ></b-form-input>
        <b-form-invalid-feedback
          v-if="password.length != 0 && validationP === false"
          :state="validationP"
        >
          Your password must be 5-12 characters long.
        </b-form-invalid-feedback>
        <b-form-valid-feedback
          v-if="password.length != 0 && validationP === true"
          :state="validationP"
        >
          Looks Good.
        </b-form-valid-feedback>
      </b-form>
    </b-modal>
    <b-modal id="my-modal2" title="Xóa tài khoản" @ok="handleDelete">
      <div class="info">
        <p>Tài Khoản : {{ acc_present.username }}</p>
        <p>Bạn có chắc chắn xóa ?</p>
      </div>
    </b-modal>
  </div>
</template>

<script>
import EventBus from "../eventbus";
import Table from "./Table";
export default {
  props: { acc_present: Object, acc: Array },
  data() {
    return {
      userName: "",
      password: "",
    };
  },
  components: { Table },
  computed: {
    validationU() {
      if (this.userName.length === 0) {
        return null;
      }
      return this.userName.length > 4 && this.userName.length < 13;
    },
    validationP() {
      if (this.password.length === 0) {
        return null;
      }
      return this.password.length > 4 && this.userName.length < 13;
    },
  },
  methods: {
    handleSubmit() {
      if (!this.checkFormValidity()) {
        return;
      }
      this.submittedNames.push(this.name);
      this.$nextTick(() => {
        this.$bvModal.hide("modal-prevent-closing");
      });
    },
    handleOk(bvModalEvt) {
      if (this.password.length == 0 || this.userName.length == 0) {
        bvModalEvt.preventDefault();
        alert("Vui lòng điền đầy đủ thông tin");
      }
      if (!this.validationP || !this.validationU) {
        bvModalEvt.preventDefault();
      } else {
        alert("Đăng ký tài khoản thành công");
        let data = {};
        data.username = this.userName;
        data.password = this.password;
        EventBus.$emit("add", data);
      }
    },
    handleDelete() {
      location.reload();
    },
  },
};
</script>

<style scoped>
.info {
  display: flex;
  flex-direction: column;
}
</style>