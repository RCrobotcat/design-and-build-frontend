<template>
  <div class="manager-container">
    <!--  头部  -->
    <div class="manager-header">
      <div class="manager-header-left">
        <img src="@/assets/imgs/logo.png" />
        <div class="title">Admin Management System</div>
      </div>

      <div class="manager-header-center">
        <el-breadcrumb separator-class="el-icon-arrow-right">
          <el-breadcrumb-item :to="{ path: '/' }">Home</el-breadcrumb-item>
          <el-breadcrumb-item :to="{ path: $route.path }">{{ $route.meta.name }}</el-breadcrumb-item>
        </el-breadcrumb>
      </div>

<!--      <div class="manager-header-right">-->
<!--        <el-dropdown placement="bottom">-->
<!--          <div class="avatar">-->
<!--            <img :src="'https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png'" />-->
<!--            <div>{{ '管理员' }}</div>-->
<!--          </div>-->
<!--          <el-dropdown-menu slot="dropdown">-->
<!--            <el-dropdown-item @click.native="goToPerson">个人信息</el-dropdown-item>-->
<!--            <el-dropdown-item @click.native="$router.push('/password')">修改密码</el-dropdown-item>-->
<!--            <el-dropdown-item @click.native="logout">退出登录</el-dropdown-item>-->
<!--          </el-dropdown-menu>-->
<!--        </el-dropdown>-->
<!--      </div>-->
    </div>

    <!--  主体  -->
    <div class="manager-main">
      <!--  侧边栏  -->
      <div class="manager-main-left">
        <el-menu :default-openeds="['info', 'user']" router style="border: none; width: 300px;" :default-active="$route.path">
          <el-menu-item index="/home">
            <i class="el-icon-s-home"></i>
            <span slot="title" style="width: 300px;">Home</span>
          </el-menu-item>
          <el-submenu index="info">
            <template slot="title">
              <i class="el-icon-menu"></i><span>Information Management</span>
            </template>
            <el-menu-item index="/notice" style="width: 300px;">Notice Information</el-menu-item>
          </el-submenu>

          <el-submenu index="user">
            <template slot="title">
              <i class="el-icon-menu"></i><span>User Management</span>
            </template>
            <el-menu-item index="/admin" style="width: 300px;">Admin Information</el-menu-item>
          </el-submenu>
        </el-menu>
      </div>

      <div class="manager-main-right">
<!--        <router-view @update:user="updateUser" />-->
        <router-view />
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "Manager",
  data() {
    return {
      // user: JSON.parse(localStorage.getItem('xm-user') || '{}'),
    }
  },
  // created() {
  //   if (!this.user.id) {
  //     this.$router.push('/login')
  //   }
  // },
  methods: {
    // updateUser() {
    //   this.user = JSON.parse(localStorage.getItem('xm-user') || '{}')   // 重新获取下用户的最新信息
    // },
    // goToPerson() {
    //   if (this.user.role === 'ADMIN') {
    //     this.$router.push('/adminPerson')
    //   }
    // },
    // logout() {
    //   localStorage.removeItem('xm-user')
    //   this.$router.push('/login')
    // }
  }
}
</script>

<style scoped>
@import "@/assets/css/manager.css";
</style>