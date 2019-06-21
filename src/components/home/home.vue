<template>
  <el-container>
    <!-- 头部 -->
    <el-header>
      <TopHead></TopHead>
    </el-header>
    <el-container>
      <!-- 侧边栏 -->
      <el-aside width="200px">
        <Slibar></Slibar>
      </el-aside>
      <!-- 内容 -->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import TopHead from "../layout/topHead";
import Slibar from "../layout/slibar";
export default {
  components: {
    TopHead: TopHead,
    Slibar: Slibar
  },
  mounted() {
    let token = window.localStorage.getItem("token");
    if (!token) {
      this.$router.push("/login");
    }
  }
};
</script>

<style>
.el-container {
  height: 100%;
}
.el-container h2 {
  margin: 0;
  color: #fff;
}
.el-header {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  line-height: 60px;
  height: 100%;
}
.el-header .mylink {
  text-decoration: none;
  color: honeydew;
}

.el-aside {
  background-color: #d3dce6;
  color: #333;
  height: 100%;
}

.el-main {
  background-color: #e9eef3;
  color: #333;
  height: 100%;
}
</style>
