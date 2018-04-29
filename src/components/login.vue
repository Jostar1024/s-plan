<template>
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      欢迎回到猿计划
    </div>
    <el-form ref="form" :rules="loginRules" :model="form" label-width="80px">
      <el-form-item label="" prop="phoneNumber">
        <el-input v-model="form.phoneNumber" placeholder="请输入手机号来注册" ></el-input>
      </el-form-item>
      <el-form-item label="" prop="password">
        <el-input type="password" v-model="form.password" placeholder="6-16位密码，区分大小写，不能使用空格" ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">登陆</el-button>
      </el-form-item>
    </el-form>
  </el-card>

</template>

<script>
import axios from 'axios'
import { setUserInfo } from '../utils/userinfo'

export default {
  data () {
    return {
      form: {
        phoneNumber: '',
        password: ''
      },
      loginRules: {
        phoneNumber: [{ required: true, trigger: 'blur', pattern: /^1[0-9]{10}$/, message: 'Invalid phone number' }],
        password: [{ required: true, trigger: 'blur', pattern: /^[^\s]{6,16}$/, message: 'Invalid password' }]
      }
    }
  },
  methods: {
    async onSubmit () {
      console.log('submit!')
      const rawRes = await axios.post('http://yjhapi.agxx.club/iweb/login/check', {
        'mobile': this.form.phoneNumber,
        'pwd': this.form.password
      })
      const res = JSON.parse(rawRes.data.substr(1))
      console.log(res)
      // 13011072992
      /*
        avatar:"http://yjhapi.agxx.club/avatar/default/8.png"
        mobile:"13011072992"
        nick_name: "测试"
        sex:0
        token:"45208f6d9e0d9990fe865065da2046ea"
        user_id:"user8"
       */
      if (res.status === 1) { // success
        this.$cookie.set('token', res.data.token, 30)
        setUserInfo(res.data)
        this.$bus.$emit('userLogin', 'ok')
        this.$router.push('/')
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
