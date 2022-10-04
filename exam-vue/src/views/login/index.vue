<template>
  <div>
    <el-form ref="postForm" :model="postForm" :rules="loginRules" class="loginContainer">
      <h3 class="loginTitle">系统登录</h3>
      <el-form-item prop="username">
        <el-input type="text" auto-complete="false" v-model="postForm.username" placeholder="请输入用户名"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input type="password" auto-complete="false" v-model="postForm.password" show-password placeholder="请输入密码"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" style="width: 100%" @click.native.prevent="accountLogin">登录</el-button>
      </el-form-item>
      <el-form-item>
        <el-form-item>
          <el-button type="primary" style="width: 100%;background-color: orangered" @click="Register">注册</el-button>
        </el-form-item>
      </el-form-item>

    </el-form>
  </div>
</template>


<script>
import { mapGetters } from 'vuex'

export default {
  data() {
    const pwdCheck = async(rule, value, callback) => {
      if (value.length < 3) {
        return callback(new Error('密码不能少于3位！'));
      } else if (value.length > 16) {
        return callback(new Error('密码最长不能超过16位！'));
      } else {
        callback()
      }
    };
    return {
      loading: false,
      postForm: {
        username: '',
        password: ''
      },
      loginRules: {
        username:[{required: true, message:'用户名不能为空',trigger: 'blur'}],
        password: [{required: true, validator: pwdCheck,trigger: 'blur'}]
      }
    }
  },
  computed: {
    ...mapGetters([
      'siteData'
    ])
  },

  methods: {

    loginBack() {
      // 其它跳到后台
      this.$router.push({ path: '/admin/dashboard' })

      setTimeout(function() {
        this.loading = false
      }, 1800)
    },

    Register(){
      console.log("hhhh")
      this.$router.push({ path: '/register' })

      setTimeout(function() {
        this.loading = false
      }, 1800)
    },

    accountLogin() {
      this.$refs.postForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.postForm)
            .then(() => {
              this.loginBack()
            })
            .catch(() => {
              this.loading = false
            })
        }
      })
    }
  }
}
</script>
<style>
  .loginContainer{
    border-radius: 15px;
    background-clip: padding-box;
    margin: 180px auto;
    width: 350px;
    padding: 15px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow:0 0 25px #cac6c6;
  }
  .loginTitle{
    margin: 0px auto 40px auto;
    text-align: center;
  }
</style>
