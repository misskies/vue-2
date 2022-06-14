<template>
  <el-container style="height: 100vh">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246);box-shadow: 2px 0 6px rgb(0,21,41)">
      <el-menu :default-openeds="['1', '3']" style="min-height: 100% ;overflow-x: hidden"
               background-color="rgb(48,65,86)"
               text-color="#fff"
               active-text-color="#ffd04b"
      >
        <div style="height: 60px;line-height: 60px;text-align: center">
          <img src="../assets/logo.png" alt="" style="width:20px">
          <b style="color: white">后台管理系统</b>
        </div>
        <el-submenu index="1">
          <template slot="title"><i class="el-icon-message"></i>人员管理</template>
        </el-submenu>

        <el-submenu index="2">
          <template slot="title"><i class="el-icon-menu"></i>货物情况</template>
        </el-submenu>

        <el-submenu index="3">
          <template slot="title"><i class="el-icon-setting">货物管理</i></template>
        </el-submenu>

      </el-menu>
    </el-aside>

    <el-container>
      <el-header style="height:10px;text-align: right; font-size: 12px">
        <el-dropdown style="width: 100px;cursor: pointer">
          <span>王小虎</span><i class="el-icon-arrow-down" style="margin-right: 15px"></i>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>个人信息</el-dropdown-item>
            <el-dropdown-item>退出</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-header>

      <el-main>
        <el-breadcrumb separator="/">
          <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
          <el-breadcrumb-item><a href="/">活动管理</a></el-breadcrumb-item>
        </el-breadcrumb>

        <div style="margin: 10px 0" >
          <el-input style="width:200px" placeholder="请输入商品名称" suffix-icon="ei-icon-search" v-model="username"></el-input>
          <el-button class="ml-5" type="primary" @click="load">搜索</el-button>
        </div>
        <div style="margin: 10px 0">
          <el-button type="primary">新增<i class="el-icon-circle-plus"></i></el-button>
        </div>

        <el-table :data="tableData" border stripe>
          <el-table-column prop="id" label="Id" width="80"></el-table-column>
          <el-table-column prop="username" label="用户名" width="140"></el-table-column>
          <el-table-column prop="nickname" label="昵称" width="120"></el-table-column>
          <el-table-column prop="email" label="邮箱"></el-table-column>
          <el-table-column prop="phone" label="电话"></el-table-column>
          <el-table-column prop="address" label="地址"></el-table-column>

          <el-table-column>
            <template slot-scope="scope">
              <el-button type="success">编辑<i class="el-icon-edit"></i></el-button>
              <el-button type="danger">删除<i class="el-icon-remove"></i></el-button>
            </template>
          </el-table-column>
        </el-table>


        <div style="padding: 10px 0">
          <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="pageNum"
              :page-sizes="[2,5, 10, 15]"
              :page-size="pageSize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="total">
          </el-pagination>
        </div>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>

export default {
  name: 'HomeView',

  data() {
    return {
      tableData: [],
      total: 0,
      pageNum:1,
      pageSize: 2,
      username:""
    }
  },
  created()
  {
    //请求分页
    this.load()
  },
  methods:{
    load()
    {
      fetch("http://localhost:9090/user/page?pageNum="+this.pageNum+"&pageSize="+this.pageSize+"&username="+this.username).then(res =>res.json()).then(res =>{
        console.log(res)
        this.tableData=res.data
        this.total=res.total
      })
    },
    handleSizeChange(pageSize)
    {
      console.log(pageSize)
      this.pageSize=pageSize
      this.load()
    } ,
    handleCurrentChange(pageNum)
    {
      console.log(pageNum)
      this.pageNum=pageNum
      this.load()
    }
  }
}
</script>

