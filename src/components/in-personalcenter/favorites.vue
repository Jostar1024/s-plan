<template>
  <el-container>
    <el-header>我的收藏</el-header>
    <el-main>
      <el-table :data="favoritesData">
        <el-table-column prop="topic" label="主题" width="400">
        </el-table-column>
        <el-table-column prop="category" label="分类">
        </el-table-column>
        <el-table-column prop="replies" label="回复">
        </el-table-column>
        <el-table-column prop="reads" label="浏览">
        </el-table-column>
        <el-table-column prop="address" label="发布时间">
        </el-table-column>
        <el-table-column prop="operation" label="操作">
        </el-table-column>
      </el-table>
    </el-main>
  </el-container>
</template>

<script>
import axios from 'axios'
import { getUserInfo } from '../../utils/userinfo'

export default {
  data () {
    return {
      favoritesData: []
    }
  },
  methods: {
    async fetchFavoritesData () {
      const rawRes = await axios.post('http://yjh.li-shang-bin.com/iweb/collect/myCollect', {
        page: 1,
        user_id: getUserInfo().user_id,
        token: this.$cookie.get('token')
      }, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })
      if (rawRes.status === 1) {
        return rawRes.data.list.data
      }
      return []
    }
  },
  mounted () {
    console.log('favorites mounted')
    this.fetchFavoritesData().then((data) => {
      this.favoritesData = data
    })
  }
}
</script>
