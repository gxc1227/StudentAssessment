<template>
  <div class="container">
    <div style="width: 400px; padding: 40px; background-color: rgba(255, 255, 255, 0.5); border-radius: 20px; box-shadow: 0 0 1000px rgba(0,0,0,0.11); backdrop-filter: blur(3px);">
      <div style="position:relative;width: 100%; text-align: center; font-size: 2.5em;font-weight: 800; margin-bottom: 10px; color: #8f2c24">注册</div>
      <el-form :model="form" :rules="rules" ref="formRef">
        <el-form-item prop="username">
          <el-input prefix-icon="el-icon-user" placeholder="请输入账号" v-model="form.username"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input prefix-icon="el-icon-lock" placeholder="请输入密码" show-password  v-model="form.password"></el-input>
        </el-form-item>
        <el-form-item prop="confirmPass">
          <el-input prefix-icon="el-icon-lock" placeholder="请确认密码" show-password  v-model="form.confirmPass"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button style="width: 100%; background-color: #333; border-color: #333; color: white" @click="register">注 册</el-button>
        </el-form-item>
        <div style="display: flex; align-items: center">
          <div style="flex: 1"></div>
          <div style="flex: 1; text-align: right">
            已有账号？请 <a href="/login">登录</a>
          </div>
        </div>
      </el-form>
    </div>
    <div class="flower">
      <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">
      <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">
      <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">
      <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">
      <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">
      <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">
      <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">
      <img src="@/assets/imgs/2.jpg" style="width: 30px; height: auto">
    </div>
  </div>
</template>

<script>
export default {
  name: "Register",
  data() {
    // 验证码校验
    const validatePassword = (rule, confirmPass, callback) => {
      if (confirmPass === '') {
        callback(new Error('请确认密码'))
      } else if (confirmPass !== this.form.password) {
        callback(new Error('两次输入的密码不一致'))
      } else {
        callback()
      }
    }
    return {
      form: {},
      rules: {
        username: [
          { required: true, message: '请输入账号', trigger: 'blur' },
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
        ],
        confirmPass: [
          { validator: validatePassword, trigger: 'blur' }
        ]
      }
    }
  },
  created() {

  },
  methods: {
    register() {
      this.$refs['formRef'].validate((valid) => {
        if (valid) {
          // 验证通过
          this.$request.post('/register', this.form).then(res => {
            if (res.code === '200') {
              this.$router.push('/')  // 跳转登录页面
              this.$message.success('注册成功')
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
  background-image: url("@/assets/imgs/3.jpg");
  background-size: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  /* color: #666; */
  overflow: hidden; /* 确保没有滚动条 */
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
a {
  color: #2a60c9;
}
</style>