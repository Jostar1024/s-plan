<template>
  <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
    <el-menu-item index="1">主页</el-menu-item>
    <el-menu-item index="2">知识体系</el-menu-item>
    <el-menu-item index="3">活动中心</el-menu-item>
    <el-menu-item index="4">文章</el-menu-item>
  </el-menu>
</template>

<script>
export default {
  data () {
    return {
      activeIndex: '1'
    }
  },
  methods: {
    handleSelect (key, keyPath) {
      console.log(key, keyPath)
    }
  }
}
</script>
