<template>
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      注册
    </div>
    <el-form ref="form" :rules="registrationRules"
    :model="form" label-width="80px">
      <el-form-item label="电话号码" prop="phoneNumber">
        <el-input v-model="form.phoneNumber" placeholder="请输入手机号来注册" ></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="form.password" placeholder="6-16位密码，区分大小写，不能使用空格"></el-input>
      </el-form-item>
      <el-form-item label="确认密码" prop="confirmPassword">
        <el-input type="password" v-model="form.confirmPassword" placeholder="请再次输入确认"></el-input>
      </el-form-item>
      <el-form-item label="手机验证码" prop="verificationCode">
        <el-input v-model="form.verificationCode" placeholder=""></el-input>
        <el-button @click="sendVCode">发送验证码</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onRegistration">注册</el-button>
      </el-form-item>
    </el-form>
  </el-card>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    const validatePassword = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('密码不能为空'))
      } else if (value !== this.form.password) {
        callback(new Error('两次密码输入不一致'))
      } else {
        callback()
      }
    }
    return {
      form: {
        phoneNumber: '',
        password: '',
        verificationCode: ''
      },
      registrationRules: {
        phoneNumber: [{ required: true, trigger: 'blur', pattern: /^1[0-9]{10}$/, message: 'Invalid phone number' }],
        password: [{ required: true, trigger: 'blur', pattern: /^[^\s]{6,16}$/, message: 'Invalid password' }],
        confirmPassword: [{ required: true, trigger: 'blur', validator: validatePassword }]
      }
    }
  },
  methods: {
    async onRegistration () {
      const rawRes = await axios.post('http://yjhapi.agxx.club/iweb/regist/index', {
        mobile: this.form.phoneNumber,
        pwd: this.form.password,
        sms_code: this.form.verificationCode
      })
      const res = JSON.parse(rawRes.data.substr(1))
      if (res.status === 1) {
        console.log('success')
      }
    },
    async sendVCode () {
      if (/^1[0-9]{10}$/.test(this.form.phoneNumber) === false) {
        return
      }
      const rawRes = await axios.post('http://yjhapi.agxx.club/iweb/Sendsms/send', {
        mobile: this.form.phoneNumber
      })
      const res = JSON.parse(rawRes.data.substr(1))
      if (res.status === 1) {
        console.log('success of sending verification code')
      }
    }
  }

}
</script>

<style>
  .text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 18px;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 480px;
  }
</style>
