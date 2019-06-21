<template>
  <el-col>
    <el-col :span="4">
      <div class="grid-content bg-purple">
        <img src="../../assets/imgs/logo.png" alt>
      </div>
    </el-col>
    <el-col :span="16">
      <div class="grid-content bg-purple-light">
        <h2>电商平台管理系统</h2>
      </div>
    </el-col>
    <el-col :span="4">
      <div class="grid-content bg-purple">
        <a class="mylink" @click.prevent="logout" href="#">退出</a>
      </div>
    </el-col>
  </el-col>
</template>

<script>
export default {
  methods: {
    logout() {
      this.$confirm("您确定要退出吗?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.$router.push("/login");
        window.localStorage.removeItem("token");
        this.$message({
          type: "success",
          message: "退出成功!"
        });
      });
    }
  }
};
</script>

<style>
</style>
