<template>
  <dir class="login-wrap">
    <el-form class="login-form" label-position="top" label-width="80px" :model="formdata">
      <h2>用户登录</h2>
      <el-form-item label="用户名">
        <el-input v-model="formdata.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="formdata.password"></el-input>
      </el-form-item>
      <el-button @click.prevent="handleLogin()" class="login-btn" type="primary">登录</el-button>
    </el-form>
  </dir>
</template>

<script>
export default {
  data() {
    return {
      formdata: {
        username: "",
        password: ""
      }
    };
  },
  methods: {
    // 发送登录请求
    async handleLogin() {
      const res = await this.$http.post(`login`, this.formdata);
      console.log(res);
      const {
        data: {
          data: { token },
          meta: { msg, status }
        }
      } = res;

      if (status === 200) {
        // 把正确的用户的token保存
        // 存值
        localStorage.setItem("token", token);
        // 取值
        // const aa = localStorage.getItem("token");
        // console.log(aa);

        // 成功时渲染home.vue
        this.$router.push({
          name: "home"
        });
      } else {
        // 提示框
        this.$message.error(msg);
      }

      // .then(res => {
      //   console.log(res);
      //   const {
      //     data: {
      //       data,
      //       meta: { msg, status }
      //     }
      //   } = res;
      //   if (status === 200) {
      //     // 成功时渲染home.vue
      //     this.$router.push({
      //       name: "home"
      //     });
      //   } else {
      //     // 提示框
      //     this.$message.error(msg);
      //   }
      // })
      // .catch(err => {
      //   console.log(err);
      // });
    }
  }
};
</script>

<style>
.login-wrap {
  height: 100%;
  background-color: #324152;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-form {
  background-color: #ffffff;
  border-radius: 5px;
  width: 400px;
  padding: 30px;
}
.login-btn {
  width: 100%;
}
</style>
