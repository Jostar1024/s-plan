<template>
  <el-menu :default-active="index" router class="el-menu-demo" mode="horizontal" @select="handleSelect">
    <el-menu-item index="/">主页</el-menu-item>
    <el-menu-item index="/knowledgesystem">知识体系</el-menu-item>
    <el-menu-item index="/activitycenter">活动中心</el-menu-item>
    <el-menu-item index="/prose">文章</el-menu-item>
    <el-menu-item index="/login" v-if="isLogged === true">登录</el-menu-item>
    <el-menu-item index="/personalcenter" v-else>个人中心</el-menu-item>
    <el-menu-item index="/register" v-if="isLogged === true">注册</el-menu-item>
    <el-menu-item index="logout" v-else @click="logout()">注销</el-menu-item>
  </el-menu>
</template>

<script>
import { deleteUserInfo } from '../utils/userinfo'
export default {
  data () {
    return {
      isLogged: this.checkLogin(),
      index: ''
    }
  },
  methods: {
    handleSelect (key, keyPath) {
      console.log(key, keyPath)
    },
    checkLogin () {
      const token = this.$cookie.get('token')
      return token === null
    },
    logout () {
      this.$cookie.delete('token')
      deleteUserInfo()
      this.handleUserLoginEvent()
      this.$router.push('/')
    },
    handleUserLoginEvent (eventData) {
      this.isLogged = this.checkLogin()
    }
  },
  created () {
    this.$bus.$on('userLogin', (eventData) => {
      this.handleUserLoginEvent(eventData)
    })
  },
  beforeDestroy () {
    this.$bus.$off('userLogin', this.handleUserLoginEvent)
  }
}
</script>

<style scoped>
li a {
  text-decoration: none;
}
</style>
