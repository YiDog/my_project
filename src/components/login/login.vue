<template>
  <div class="app">
    <div class="mybox">
      <el-form label="left" :model="formObj" :rules="rules" ref="ruleForm">
        <h1>用户登录</h1>
        <el-form-item label="账号" prop="username">
          <el-input type="text" v-model="formObj.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="password" v-model="formObj.password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" class="mylink" @click.prevent="loginFn">提交</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formObj: {
        username: "",
        password: ""
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }]
      }
    };
  },

  methods: {
    loginFn() {
      this.$refs.ruleForm.validate(valid => {
        if (valid) {
          this.$http({
            method: "POST",
            url: "http://localhost:8888/api/private/v1/login",
            data: this.formObj
          }).then(res => {
            console.log(res);
            // 解构
            let { data, meta } = res.data;
            if (meta.status === 200) {
              this.$router.push("/");
              localStorage.setItem("token", data.token);
              this.$message({
                message: "恭喜您，登录成功",
                type: "success"
              });
            }
          });
        } else {
          this.$message.error("错了哦，您的账号或密码错误");
        }
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
.app {
  height: 100%;
  width: 100%;
  background-color: #324252;
  position: relative;
}
.mybox {
  width: 580px;
  height: 440px;
  background-color: #fff;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 40px;
  box-sizing: border-box;
}
.mylink {
  width: 100%;
}
</style>
