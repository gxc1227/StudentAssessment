<template>
  <div class="manager-container">
    <!--  头部  -->
    <div class="manager-header">
      <div class="manager-header-left">
        <img src="@/assets/imgs/logo.png" />
        <div class="title">大学生综合行为测评系统</div>
      </div>

      <div class="manager-header-center">
        <el-breadcrumb separator-class="el-icon-arrow-right">
          <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
          <el-breadcrumb-item :to="{ path: $route.path }">{{ $route.meta.name }}</el-breadcrumb-item>
        </el-breadcrumb>
      </div>

      <div class="manager-header-right">
        <el-dropdown placement="bottom">
          <div class="avatar">
            <img :src="user.avatar || 'https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png'" />
            <div>{{ user.name ||  '管理员' }}</div>
          </div>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item @click.native="goToPerson">个人信息</el-dropdown-item>
            <el-dropdown-item @click.native="$router.push('/password')">修改密码</el-dropdown-item>
            <el-dropdown-item @click.native="logout">退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </div>

    <!--  主体  -->
    <div class="manager-main">
      <!--  侧边栏  -->
      <div class="manager-main-left">
        <!-- 将 default-openeds 属性的值设为空数组 -->
        <el-menu :default-openeds="[]" router style="border: none" :default-active="$route.path">
          <el-menu-item index="/home">
            <i class="el-icon-s-home"></i>
            <span slot="title">系统首页</span>
          </el-menu-item>
          <el-submenu index="info">
            <template slot="title">
              <i class="el-icon-menu"></i><span>信息管理</span>
            </template>
            <el-menu-item index="/notice">公告信息</el-menu-item>
          </el-submenu>
          <el-submenu index="system">
            <template slot="title">
              <i class="el-icon-menu"></i><span>系统管理</span>
            </template>
            <el-menu-item index="/admin">管理员管理</el-menu-item>
            <el-menu-item index="/user">用户管理</el-menu-item>
            <el-menu-item index="/role">角色管理</el-menu-item>
            <el-menu-item index="/permission">权限管理</el-menu-item>
          </el-submenu>
          <el-submenu index="course">
            <template slot="title">
              <i class="el-icon-menu"></i><span>课程管理</span>
            </template>
            <el-menu-item index="/course-create">课程创建</el-menu-item>
            <el-menu-item index="/task-issue">任务下发</el-menu-item>
            <el-menu-item index="/course-activate">课程激活</el-menu-item>
          </el-submenu>
          <el-submenu index="assessment">
            <template slot="title">
              <i class="el-icon-menu"></i><span>评估管理</span>
            </template>
            <el-menu-item index="/assessment-dimension">评估维度管理</el-menu-item>
            <el-menu-item index="/assessment-task">评估任务管理</el-menu-item>
            <el-menu-item index="/assessment-result">评估结果查看</el-menu-item>
          </el-submenu>
          <el-submenu index="score">
            <template slot="title">
              <i class="el-icon-menu"></i><span>成绩管理</span>
            </template>
            <el-menu-item index="/score-input">成绩录入</el-menu-item>
            <el-menu-item index="/score-query">成绩查询</el-menu-item>
            <el-menu-item index="/score-statistics">成绩统计</el-menu-item>
          </el-submenu>
          <el-submenu index="comprehensive">
            <template slot="title">
              <i class="el-icon-menu"></i><span>综测管理</span>
            </template>
            <el-menu-item index="/material-upload">材料上传</el-menu-item>
            <el-menu-item index="/score-feedback">成绩反馈</el-menu-item>
            <el-menu-item index="/proof-audit">证明审核</el-menu-item>
          </el-submenu>
          <el-submenu index="analysis">
            <template slot="title">
              <i class="el-icon-menu"></i><span>数据分析</span>
            </template>
            <el-menu-item index="/academic-analysis">学业分析</el-menu-item>
            <el-menu-item index="/comprehensive-analysis">综测分析</el-menu-item>
            <el-menu-item index="/comprehensive-report">综合报告</el-menu-item>
          </el-submenu>
        </el-menu>
      </div>

      <!--  数据表格  -->
      <div class="manager-main-right">
        <router-view @update:user="updateUser" />
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: "Manager",
  data() {
    return {
      user: JSON.parse(localStorage.getItem('xm-user') || '{}'),
    }
  },
  created() {
    if (!this.user.id) {
      this.$router.push('/login')
    }
  },
  methods: {
    updateUser() {
      this.user = JSON.parse(localStorage.getItem('xm-user') || '{}')   // 重新获取下用户的最新信息
    },
    goToPerson() {
      if (this.user.role === 'ADMIN') {
        this.$router.push('/adminPerson')
      }
    },
    logout() {
      localStorage.removeItem('xm-user')
      this.$router.push('/login')
    }
  }
}
</script>

<style scoped>
@import "@/assets/css/manager1.css";
</style>