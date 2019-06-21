<template>
  <el-card>
    <!-- 面包屑导航栏 -->
    <el-breadcrumb>
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 搜索框&新增按钮 -->
    <el-row>
      <el-col :span="7">
        <el-input v-model="query" placeholder="请输入内容" class="myinput">
          <el-button slot="append" icon="el-icon-search" @click.prevent="searchFn"></el-button>
        </el-input>
      </el-col>
      <el-col :span="4">
        <el-button type="success" plain class="mybtn" @click.prevent="adduser">新增用户</el-button>
      </el-col>
    </el-row>
    <!-- 表格栏 -->
    <el-table style="width: 100%" :data="tableData">
      <el-table-column type="index"></el-table-column>
      <el-table-column prop="username" v-model="tableData.username" label="用户" width="180"></el-table-column>
      <el-table-column prop="email" v-model="tableData.email" label="邮箱" width="180"></el-table-column>
      <el-table-column prop="mobile" v-model="tableData.mobile" label="地址"></el-table-column>
      <el-table-column label="用户状态">
        <template slot-scope="scope">
          <!-- {{scope.row}} -->
          <el-switch v-model="scope.row.mg_state" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template>
          <el-button type="primary" icon="el-icon-edit" plain size="small"></el-button>
          <el-button type="danger" icon="el-icon-delete" plain size="small"></el-button>
          <el-button type="success" icon="el-icon-check" plain size="small"></el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页栏 -->
    <el-pagination
      @current-change="curreChange"
      @size-change="sizeChange"
      :current-page="pagenum"
      :page-sizes="pagesizes"
      :page-size="pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    ></el-pagination>
    <!-- 自定义弹出表单 -->

    <el-dialog title="添加用户" :visible.sync="dialogFormVisible">
      <!-- {{formObj}} -->
      <el-form :rules="rules" :model="formObj">
        <el-form-item label="用户名" prop="username" :label-width="formLabelWidth">
          <el-input type="text" v-model="formObj.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password" :label-width="formLabelWidth">
          <el-input type="password" v-model="formObj.password"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" :label-width="formLabelWidth">
          <el-input type="email" v-model="formObj.email"></el-input>
        </el-form-item>
        <el-form-item label="电话" :label-width="formLabelWidth">
          <el-input type="text" v-model="formObj.mobile"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click.prevent="adduserFn">确 定</el-button>
      </div>
    </el-dialog>
  </el-card>
</template>

<script>
export default {
  data() {
    return {
      tableData: [],
      query: "",
      pagenum: 1,
      pagesize: 5,
      pagesizes: [5, 10, 20],
      total: 0,
      dialogFormVisible: false,
      formLabelWidth: "80px",
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }]
      },
      formObj: {
        username: "",
        password: "",
        email: "",
        mobile: ""
      }
    };
  },
  methods: {
    getTableData() {
      this.$http({
        method: "GET",
        url: `http://localhost:8888/api/private/v1/users/?query=${
          this.query
        }&pagenum=${this.pagenum}&pagesize=${this.pagesize}`,
        headers: { Authorization: window.localStorage.getItem("token") }
      }).then(res => {
        // console.log(res);
        // 结构
        let { data, meta } = res.data;
        if (meta.status === 200) {
          //   console.log(data.users);
          this.tableData = data.users;
          this.total = data.total;
        }
      });
    },
    curreChange(currentPage) {
      //   console.log(currentPage);
      this.pagenum = currentPage;
      this.getTableData();
    },
    sizeChange(pagesize) {
      //   console.log(pagesize);
      this.pagesize = pagesize;
      this.getTableData();
    },
    searchFn() {
      this.$http({
        method: "GET",
        url: `http://localhost:8888/api/private/v1/users/?query=${
          this.query
        }&pagenum=${this.pagenum}&pagesize=${this.pagesize}`,
        headers: { Authorization: window.localStorage.getItem("token") }
      }).then(res => {
        if (res.data.meta.status === 200) {
          this.tableData = res.data.data.users;
          this.getTableData();
        }
      });
    },
    adduser() {
      this.dialogFormVisible = true;
    },
    adduserFn() {
      this.$http({
        method: "POST",
        url: "http://localhost:8888/api/private/v1/users",
        data: this.formObj,
        headers: { Authorization: window.localStorage.getItem("token") }
      }).then(res => {
        console.log(res);
        if (res.data.meta.status === 201) {
          this.dialogFormVisible = false;
          this.formObj.email = "";
          this.formObj.mobile = "";
          this.formObj.username = "";
          this.formObj.password = "";
          this.getTableData();
        }
      });
    }
  },
  mounted() {
    this.getTableData();
  }
};
</script>

<style>
.myinput {
  margin-top: 15px;
}
.mybtn {
  margin-top: 15px;
  margin-left: 8px;
}
</style>
