<template>
  <div id="login" @keyup.enter="submitForm()">
    <el-card class="box-card">
        <h3 class="login-title">运营统计系统</h3>
        <el-form ref="form" :model="form">
          <el-form-item>
            <el-input v-model="form.username" placeholder="帐号" type="text">{{form.username}}</el-input>
          </el-form-item>
          <el-form-item>
            <el-input v-model="form.password" placeholder="密码" type="password">{{form.password}}</el-input>
          </el-form-item>
          <el-form-item>
              <el-button class="login-button" type="warning" @click="submitForm">登录</el-button>
          </el-form-item>
        </el-form>
    </el-card>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'login',
  data () {
    return {
      form: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    submitForm () {
      const path = this.$store.state.url + '/api/signin'
      axios.post(path, {username: this.form.username, password: this.form.password})
        .then(response => {
          if (response.data.status === 'ok') {
            sessionStorage.setItem('username', response.data.username)
            sessionStorage.setItem('id', 'mdhz')
            this.$store.commit('increment', true)
            this.$router.push({path: '/tableList'})
          } else {
            this.form.password = ''
            this.$message.error(response.data.status)
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  }
}
</script>

<style scoped>
  #login {
  height: 100vh;
  width: 100%;
  }
  #login .box-card {
  width: 400px;
  position: absolute;
  left:50%; top:50%;
  transform:translate(-50%,-50%);
  }
  .login-title {
      line-height: 36px;
      color: #FF9800;
      margin: 10px 0;
      text-align: center;
  }
  .login-button {
      width: 100%;
  }
</style>
