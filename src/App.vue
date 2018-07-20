<template>
  <div id="app">
    <home-header :seller="seller"></home-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <!-- 路由出口 -->
    <router-view></router-view>
  </div>
</template>

<script>
import HomeHeader from './components/header/header'

// 将ajax请求的返回状态码抽取出来，方便查阅和维护
const ERR_OK = 0

export default {
  name: 'App',
  components: {
    HomeHeader
  },
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$http({
      method: 'get',
      url: '/api/seller'
    }).then((res) => {
      res = res.data
      if (res.errno === ERR_OK) {
        this.seller = res.data
      }
    }).catch((err) => {
      console.log(err)
    })
  }
}
</script>

<style lang="stylus">
.tab
  display: flex
  width: 100%
  height: 40px
  line-height: 40px
  border-1px(rgba(7, 17, 27, .1))
  .tab-item
    flex: 1
    text-align: center
    & > a
      display: block
      font-size: 14px
      color: rgb(77, 85, 93)
      &.active
        color: rgb(240, 20, 20)
</style>
