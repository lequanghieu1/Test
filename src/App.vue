<template>
  <div id="app">
    <login :acc="acc" />
  </div>
</template>

<script>
import login from "./components/Login.vue";
import EventBus from "./eventbus";
export default {
  name: "App",
  components: {
    login,
  },
  data() {
    return {
      acc: [
        {
          id: 1,
          username: "user1",
          pass: "123",
          fullname: "Nguyễn Văn A",
          age: 20,
          email: "a@gmail.com",
          role:'Admin'
        },
        {
          id: 2,
          username: "user2",
          pass: "123",
          fullname: "Nguyễn Văn B",
          age: 10,
          email: "b@gmail.com",
          role:'User'
        },
      ],
    };
  },
  created() {
    EventBus.$on("delete", (data) => {
      this.acc = this.acc.filter((element) => element.id != data.id);
    });
    EventBus.$on("add", (data) => {
      data.email = '123@gmail.com'
      data.fullname = 'Nguyễn Văn C'
      data.age = 23
      this.acc.push(data);
      console.log(data)
    });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
