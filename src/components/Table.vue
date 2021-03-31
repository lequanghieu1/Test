<template>
  <div>
    <b-table :items="acc" :fields="fields" :contenteditable="is_edit">
      <template #cell(name)="row">
        {{ row.value.first }} {{ row.value.last }}
      </template>

      <template #cell(actions)="row">
        <b-button
          size="sm"
          variant="warning"
          class="mr-1"
          @click="handleUpdate(row.item)"
        >
          Sửa tài khoản
        </b-button>
        <b-button
          variant="danger"
          size="sm"
          v-b-modal="'my-modal2'"
          @click="handleDelete(row.item)"
          >Xóa tài khoản</b-button
        >
      </template>

      <template #row-details="row">
        <b-card>
          <ul>
            <li v-for="(value, key) in row.item" :key="key">
              {{ key }}: {{ value }}
            </li>
          </ul>
        </b-card>
      </template>
    </b-table>
      <b-modal id="bv-modal-example" title="BootstrapVue" size='sm'>
    <p class="my-4">Hãy nhấp vào bảng và sửa dữ liệu </p>
  </b-modal>
  </div>
</template>
<script>
import EventBus from "../eventbus";
export default {
  props: { acc: Array },
  data() {
    return {
      fields: [
        {
          key: "fullname",
          label: "Họ Tên ",
        },
        {
          key: "age",
          label: "Tuổi",
        },
        {
          key: "email",
          label: "Email",
        },
        {
          key: "username",
          label: "Tài Khoản",
        },
        { key: "actions", label: "Actions" },
      ],
      is_edit:false
    };
  },
  methods: {
    handleDelete(data) {
      this.$bvModal
        .msgBoxConfirm("Are you sure?", {
          title: "Please Confirm",
          size: "sm",
          buttonSize: "sm",
          okVariant: "danger",
          okTitle: "YES",
          cancelTitle: "NO",
          footerClass: "p-2",
          hideHeaderClose: false,
          centered: true,
        })
        .then(() => {
          EventBus.$emit("delete", data);
        });
    },
    handleUpdate() {
      this.$bvModal.show('bv-modal-example')
      this.is_edit = true;
    },
  },
};
</script>