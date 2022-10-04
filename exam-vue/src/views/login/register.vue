<template>
  <div>
    <el-form ref="postForm" :model="postForm" :rules="rules" class="registerContainer">
      <h3 class="registerTitle">用户注册</h3>
      <el-form-item prop="username">
        <el-input type="text" auto-complete="false" v-model="postForm.userName" placeholder="请输入用户名"></el-input>
      </el-form-item>
      <el-form-item prop="name">
        <el-input type="text" auto-complete="false" v-model="postForm.realName" placeholder="请输入姓名"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input type="password" auto-complete="false" v-model="postForm.password" show-password placeholder="请输入密码"></el-input>
      </el-form-item>
      <el-form-item prop="againPassword">
        <el-input type="password" auto-complete="false" v-model="postForm.againPassword" show-password placeholder="请重复密码"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" style="width: 100%" @click="handleReg">注册</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" style="width: 100%;background-color: orangered" @click="Login">返回登陆登录界面</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>


<script>
import { mapGetters } from 'vuex'

export default {
  data() {
    const pwdCheck = async(rule, value, callback) => {
      if (value.length < 6) {
        return callback(new Error('密码不能少于6位！'));
      } else if (value.length > 16) {
        return callback(new Error('密码最长不能超过16位！'));
      } else {
        callback()
      }
    };
    // 重复密码验证
    const pwdAgainCheck = async(rule, value, callback) => {
      if (value.length < 1) {
        return callback(new Error('重复密码不能为空！'));
      } else if(this.postForm.password != this.postForm.againPassword){
        return callback(new Error('两次输入密码不一致！'));
      }else{
        callback()
      }
    };
    return {
      postForm: {
        userName: '',
        realName: '',
        password: '',
        againPassword: '',

      },
      rules: {
        userName:[{required: true, message:'用户名不能为空',trigger: 'blur'}],
        password: [{required: true, validator: pwdCheck,trigger: 'blur'}],
        againPassword: [{required: true, validator: pwdAgainCheck,trigger: 'blur'}]
      },
      loading: false
    }
  },
  computed: {
    ...mapGetters([
      'siteData'
    ])
  },

  methods: {

    handleReg() {
      this.$refs.postForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/reg', this.postForm)
            .then(() => {
              this.$router.push({ path: this.redirect || '/admin/dashboard' })
              this.loading = false
            })
            .catch(() => {
              this.loading = false
            })
        }
      })
    },
    Login(){
      this.$router.replace('/login');
    }

  }
}
</script>
<style>
  .registerContainer{
    border-radius: 15px;
    background-clip: padding-box;
    margin: 180px auto;
    width: 350px;
    padding: 15px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow:0 0 25px #cac6c6;
  }
  .registerTitle{
    margin: 0px auto 40px auto;
    text-align: center;
  }
</style>
