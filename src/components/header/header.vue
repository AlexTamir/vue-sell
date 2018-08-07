<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="商家头像">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="supports-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="头部背景图" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="supports-item" v-for="(item, index) in seller.supports" :key="index">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import star from '@/components/star/star'

export default {
  name: 'HomeHeader',
  components: {
    star
  },
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
      detailShow: false
    }
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    hideDetail () {
      this.detailShow = false
    }
  }
  // created () {
  //   this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  // }
}
</script>

<style lang="stylus" scoped>
.header
  position: relative
  overflow: hidden
  color: #fff
  background-color: rgba(7, 17, 27, 0.5)
  .content-wrapper
    position: relative
    padding: 24px 12px 18px 24px
    font-size: 0
    .avatar
      display: inline-block
      vertical-align: top
      img
        border-radius: 2px
    .content
      display: inline-block
      margin-left: 16px
      .title
        margin: 2px 0 8px 0
        .brand
          display: inline-block
          vertical-align: top
          width: 30px
          height: 18px
          bg-image('brand')
          background-size: 30px 18px
          background-repeat: no-repeat
        .name
          margin-left: 6px
          font-size: 16px
          font-weight: bold
          line-height: 18px
      .description
        margin-bottom: 10px
        line-height: 12px
        font-size: 12px
      .supports
        margin-bottom: 2px
        .icon
          display: inline-block
          vertical-align: top
          width: 12px
          height: 12px
          margin-right: 4px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-image('decrease_1')
          &.discount
            bg-image('discount_1')
          &.guarantee
            bg-image('guarantee_1')
          &.invoice
            bg-image('invoice_1')
          &.special
            bg-image('special_1')
        .text
          // 14
          line-height: 12px
          font-size: 10px
    .supports-count
      position: absolute
      right: 12px
      bottom: 14px
      padding: 0 8px
      height: 24px
      // 26
      line-height: 24px
      border-radius: 14px
      background-color: rgba(0, 0, 0, .2)
      text-align center
      .count
        vertical-align: top;
        font-size: 10px
      .icon-keyboard_arrow_right
        margin-left: 2px
        line-height: 24px
        font-size: 10px
  .bulletin-wrapper
    position: relative
    height: 28px
    line-height: 28px
    padding: 0 22px 0 12px
    overflow: hidden
    white-space: nowrap
    text-overflow: ellipsis
    background-color: rgba(7, 17, 27, .2)
    .bulletin-title
      display: inline-block
      vertical-align: top
      width: 22px
      height: 12px
      // 7
      margin-top: 8px
      bg-image('bulletin')
      background-size: 22px 12px
      background-repeat: no-repeat
    .bulletin-text
      vertical-align: top
      margin: 0 4px
      font-size: 10px
    .icon-keyboard_arrow_right
      position: absolute
      // 9
      top: 8px
      right: 12px
      font-size: 10px
  .background
    position: absolute
    top: 0
    left: 0
    width: 100%
    height: 100%
    z-index: -1
    filter: blur(10px)
  .detail
    // CSS Sticky footer 布局
    position: fixed
    top: 0
    left: 0
    overflow: auto
    width: 100%
    height: 100%
    z-index: 100
    // 只有 ios 才生效
    backdrop-filter: blur(10px)
    background: rgba(7, 17, 27, .8)
    &.fade-enter-active,
    &.fade-leave-active
      transition: all .5s;
    &.fade-enter,
    &.fade-leave-to
      opacity: 0;
    .detail-wrapper
      overflow: auto
      min-height: 100%;
      .detail-main
        // 这里用 margin 没有出现预期的结果，是因为外边距重叠的原因
        // 第一，使用 padding-top: 64px
        // 第二，在 detail-wrapper 那里加上 overflow: auto
        margin-top: 64px
        // 这个是为下面的 × 留出来的位置
        padding-bottom: 128px
        .name
          line-height: 16px
          font-size: 16px
          font-weight: 700
          text-align: center
          color: rgb(255, 255, 255)
        .star-wrapper
          margin-top: 16px
          padding: 2px 0
          text-align: center
        .title
          display: flex
          width 80%
          margin: 28px auto 24px
          .line
            flex: 1
            position: relative
            top: -6px
            border-bottom: 1px solid rgba(255, 255, 255, .2)
          .text
            line-height: 14px
            padding: 0 12px
            font-size: 14px
            font-weight: 700
            color: rgb(255, 255, 255)
        .supports
          width: 80%
          margin: 0 auto
          .supports-item
            margin-bottom: 12px
            padding: 0 12px
            font-size: 0
            &:last-child
              margin-bottom: 0
            .icon
              display: inline-block
              vertical-align: top
              width: 16px
              height: 16px
              margin-right: 6px
              background-size: 16px 16px
              background-repeat: no-repeat
              &.decrease
                bg-image('decrease_2')
              &.discount
                bg-image('discount_2')
              &.guarantee
                bg-image('guarantee_2')
              &.invoice
                bg-image('invoice_2')
              &.special
                bg-image('special_2')
            .text
              line-height: 16px
              font-size: 12px
              font-weight: 200
              color: rgb(255, 255, 255)
        .bulletin
          width: 80%
          margin: 0 auto
          .content
            padding: 0 12px
            line-height: 24px
            font-size: 12px
            font-weight: 200
            color: rgb(255, 255, 255)
    .detail-close
      position: relative
      width: 32px
      height: 32px
      margin: -64px auto 0;
      font-size: 32px
</style>
