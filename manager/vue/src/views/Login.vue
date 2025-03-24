<template>
  <div class="main">
    <div ref="vantaRef"  class="vanta-background"></div>
    <div class="container">
      <div style="width: 400px; padding: 40px; background-color: rgba(255, 255, 255, 0.5); border-radius: 20px; box-shadow: 0 0 1000px rgba(0,0,0,0.11); backdrop-filter: blur(3px);">
        <div style="position:relative;width: 100%; text-align: center; font-size: 2.5em;font-weight: 800; margin-bottom: 10px; color: #8f2c24">Sign In</div>
        <!--        <el-form-item prop="username" :rules="[-->
        <!--								{required: true,message: '请输入用户名',trigger: 'blur'},-->
        <!--								{pattern: /^[a-zA-Z]\w{4,17}$/,message: '用户名式有误，请重新输入',trigger: 'blur'}]">-->
        <!--          <el-input v-model="state.loginForm.username" prefix-icon="Avatar" placeholder="请输入用户名称" style="width:100%"></el-input>-->
        <!--        </el-form-item>-->
        <!--        <el-form-item prop="password" :rules="[-->
        <!--								{required: true,message: '请输入密码',trigger: 'blur'},-->
        <!--								{pattern: /^[a-zA-Z]\w{5,17}$/,message: '密码格式有误，请重新输入',trigger: 'blur'}]">-->
        <!--          <el-input type="password" v-model="loginForm.password" prefix-icon="Lock" show-password placeholder="请输入密码" style="width:100%"></el-input>-->
        <!--        </el-form-item>-->
        <el-form :model="form" :rules="rules" ref="formRef">
          <el-form-item prop="username" style="position: relative">
            <!--          style="position: relative;width:100%;padding: 15px 20px;outline: none;font-size: 1.25em;color: #8f2c24;border-radius: 5px;background: #fff;border: none;margin-bottom: 30px"-->
            <el-input  prefix-icon="el-icon-user" placeholder="请输入账号" v-model="form.username"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input prefix-icon="el-icon-lock" placeholder="请输入密码" show-password  v-model="form.password"></el-input>
          </el-form-item>
<!--          <el-form-item prop="role">-->
<!--            <el-radio-group v-model="form.role">-->
<!--              <el-radio label="ADMIN">管理员</el-radio>-->
<!--              <el-radio label="BUSINESS">商家</el-radio>-->
<!--            </el-radio-group>-->
<!--          </el-form-item>-->
          <el-form-item>
            <el-button style="width: 100%; background-color: orangered; border-color: orangered; color: white" @click="login">登 录</el-button>
          </el-form-item>.
          <div style="display: flex; align-items: center">
            <div style="flex: 1; text-align: right">
              <a href="/register">注册账号</a>
            </div>
          </div>
        </el-form>
      </div>
      <!--      <div class="flower">-->
      <!--        <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">-->
      <!--        <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">-->
      <!--        <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">-->
      <!--        <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">-->
      <!--        <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">-->
      <!--        <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">-->
      <!--        <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">-->
      <!--        <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">-->
      <!--      </div>-->
    </div>
  </div>
<!--  <div class="container">-->
<!--    <div style="width: 400px; padding: 30px; background-color: white; border-radius: 5px;">-->
<!--      <div style="text-align: center; font-size: 20px; margin-bottom: 20px; color: #333">欢迎使用</div>-->
<!--      <el-form :model="form" :rules="rules" ref="formRef">-->
<!--        <el-form-item prop="username">-->
<!--          <el-input prefix-icon="el-icon-user" placeholder="请输入账号" v-model="form.username"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item prop="password">-->
<!--          <el-input prefix-icon="el-icon-lock" placeholder="请输入密码" show-password  v-model="form.password"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item>-->
<!--          <el-button style="width: 100%; background-color: #333; border-color: #333; color: white" @click="login">登 录</el-button>-->
<!--        </el-form-item>-->
<!--&lt;!&ndash;        <div style="display: flex; align-items: center">&ndash;&gt;-->
<!--&lt;!&ndash;          <div style="flex: 1"></div>&ndash;&gt;-->
<!--&lt;!&ndash;          <div style="flex: 1; text-align: right">&ndash;&gt;-->
<!--&lt;!&ndash;            还没有账号？请 <a href="/register">注册</a>&ndash;&gt;-->
<!--&lt;!&ndash;          </div>&ndash;&gt;-->
<!--&lt;!&ndash;        </div>&ndash;&gt;-->
<!--      </el-form>-->
<!--    </div>-->
<!--  </div>-->
</template>

<script>
import * as THREE from 'three'
import BIRDS from '@/assets/back.js'
export default {
  name: "Login",
  data() {
    return {
      form: { role: 'ADMIN' },
      rules: {
        username: [
          { required: true, message: '请输入账号', trigger: 'blur' },
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
        ]
      }
    }
  },
  created() {

  },
  mounted() {
    this.vantaEffect = BIRDS({
      el: this.$refs.vantaRef,
      THREE: THREE
    })
  },
  beforeDestroy() {
    if (this.vantaEffect) {
      this.vantaEffect.destroy()
    }
  },
  methods: {
    login() {
      this.$refs['formRef'].validate((valid) => {
        if (valid) {
          // 验证通过
          this.$request.post('/login', this.form).then(res => {
            if (res.code === '200') {
              localStorage.setItem("xm-user", JSON.stringify(res.data))  // 存储用户数据
              this.$router.push('/')  // 跳转主页
              this.$message.success('登录成功')
            } else {
              this.$message.error(res.msg)
            }
          })
        }
      })
    }
  }
}
</script>

<style scoped>
/*.container {*/
/*  height: 100vh;*/
/*  overflow: hidden;*/
/*  background-image: url("@/assets/imgs/bg.jpg");*/
/*  background-size: 100%;*/
/*  display: flex;*/
/*  align-items: center;*/
/*  justify-content: center;*/
/*  color: #666;*/
/*}*/
/*a {*/
/*  color: #2a60c9;*/
/*}*/
.main{
  width: 100%;
  height: 100%;
}
.container {
  position: relative;
  top: 60%;
  left: 0%;
  z-index: 2;
  margin: 0;
  padding: 0;
  /*height: 90vh;*/
  height: 100vh;
  /*width: 190vh;*/
  overflow: hidden;
  /*background-image: url("@/assets/imgs/3.jpg");*/
  background-size: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  /* color: #666; */
  overflow: hidden; /* 确保没有滚动条 */
}
.vanta-background {
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 1; /* 使其位于 container 背后 */
  margin: 0;
  padding: 0;
}
a {
  color: #2a60c9;
}
.background-wrapper {
  position: relative;
  width: 100%;
  height: 100vh;
  margin: 0;
  padding: 0;
}
.flower{
  position: absolute;
  width: 100%;
  height: 100vh;
  overflow: hidden;
  z-index: 1;
  pointer-events: none;
}
.flower img{
  position: absolute;
}
.flower img:nth-child(1){
  left: 20%;
  animation: animate 20s linear infinite;
}
.flower img:nth-child(2){
  left: 50%;
  animation: animate 14s -2s linear infinite;
}
.flower img:nth-child(3){
  left: 70%;
  animation: animate 12s -3s linear infinite;
}
.flower img:nth-child(4){
  left: 5%;
  animation: animate 15s -2s linear infinite;
}
.flower img:nth-child(5){
  left: 85%;
  animation: animate 18s -1s linear infinite;
}
.flower img:nth-child(6){
  left: 90%;
  animation: animate 12s -1s linear infinite;
}
.flower img:nth-child(7){
  left: 15%;
  animation: animate 14s -2s linear infinite;
}
.flower img:nth-child(8){
  left: 60%;
  animation: animate 15s -1s linear infinite;
}
@keyframes animate {
  0%{
    opacity: 0;
    top:-10px;
    transform: translateX(20px) rotate(0deg);
  }
  10%{
    opacity: 1;
  }
  20%{
    transform: translateX(-20px) rotate(45deg);
  }
  40%{
    transform: translateX(-20px) rotate(90deg);
  }
  60%{
    transform: translateX(20px) rotate(180deg);
  }
  80%{
    transform: translateX(-20px) rotate(45deg);
  }
  100%{
    top:110%;
    transform: translateX(20px) rotate(225deg);
  }
}
</style>