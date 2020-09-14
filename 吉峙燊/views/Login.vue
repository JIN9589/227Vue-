<template>
  <div>
    <el-card class="box-card">
      <h1>W227附属幼儿园</h1>
      <h4>登陆后浏览</h4>
      <p><el-input v-model="user.username" placeholder="请输入用户名" /></p>
      <p><el-input v-model="user.pwd" placeholder="请输入密码" type="password" /></p>
      <p><el-button type="primary" @click="userLogin">登录</el-button></p>
      </el-card>
  </div>
</template>
<script>
export default {
  name: 'Login',
  data: function () {
    return {
      user: {
        username: '',
        pwd: ''
      }
    }
  },
  methods: {
    userLogin () {
      if (this.user.username.length === 0) {
        this.$message({
          showClose: true,
          message: '请输入用户名',
          type: 'warning'
        })
      } else if (this.user.pwd.length === 0) {
        this.$message({
          showClose: true,
          message: '请输入密码',
          type: 'warning'
        })
      } else {
        this.$axios.post('/api/userLogin', { UserName: this.user.username, Pwd: this.user.pwd })
          .then((data) => {
            if (data.data.message === '登陆成功') {
              this.$router.push('home')
            } else {
              this.$alert(data.data.message)
            }
          }).catch((err) => {
            console.log(err)
            this.$message.error('请求出错')
          })
      }
    }
  }
}
</script>
<style scoped>
.box-card{
    width: 480px;
    height: 380px;
    margin: auto;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
}
.box-card p{
    margin-top: 20px;
}
.box-card h1{
    margin-top: 50px;
}
.box-card h4{
    margin-top: 30px;
}
</style>
