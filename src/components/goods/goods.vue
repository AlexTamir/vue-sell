<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item, index) in goods" :key="index" @click="selectMenu(index)" class="menu-item" :class="{'current': currentIndex === index}">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodWrapper">
      <ul>
        <li v-for="(item, index) in goods" :key="index" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="(food, subIndex) in item.foods" :key="subIndex" class="food-item border-1px">
              <div class="icon">
                <img :src="food.icon" :alt="food.name" width="57" height="57">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p v-if="food.description" class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span><span class="old" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
// 将ajax请求的返回状态码抽取出来，方便查阅和维护
const ERR_OK = 0

export default {
  name: 'goods',
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
      goods: [],
      listHeight: [],
      scrollY: 0
    }
  },
  computed: {
    currentIndex () {
      for (let i = 0, len = this.listHeight.length; i < len; i++) {
        let height1 = this.listHeight[i]
        let height2 = this.listHeight[i + 1]
        // i 如果遍历到最后就会返回 undefined，所以这里做下处理
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
    }
  },
  created () {
    this.$http({
      method: 'get',
      url: '/api/goods'
    }).then(res => {
      res = res.data
      if (res.errno === ERR_OK) {
        this.goods = res.data
        this.$nextTick(() => {
          this._initScroll()
          this._calculateHeight()
        })
      }
    }).catch(err => {
      console.log(err)
    })
  },
  methods: {
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {})
      this.foodScroll = new BScroll(this.$refs.foodWrapper, { probeType: 3 })
      this.foodScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    _calculateHeight () {
      let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook')
      let height = 0
      this.listHeight.push(height)
      for (let i = 0, len = foodList.length; i < len; i++) {
        let item = foodList[i]
        height += item.scrollHeight
        this.listHeight.push(height)
      }
    },
    selectMenu (index) {
      console.log(index)
    }
  }
}
</script>

<style lang="stylus" scoped>
.goods
  display: flex;
  position: absolute;
  top: 175px;
  bottom: 48px;
  width: 100%;
  overflow: hidden;
  .menu-wrapper
    flex: 0 0 80px;
    width: 80px;
    background: #f3f5f7;
    .menu-item
      display: table;
      width: 56px;
      height: 54px;
      line-height: 14px;
      padding: 0 12px
      &.current
        position: relative
        z-index: 10
        margin-top: -1px
        background: #fff
        font-weight: 700
        .text
          border-none()
      .icon
        display: inline-block;
        vertical-align: top;
        width: 12px;
        height: 12px;
        margin-right: 2px;
        background-size: 12px 12px;
        background-repeat: no-repeat;
        &.decrease
          bg-image('decrease_3');
        &.discount
          bg-image('discount_3');
        &.guarantee
          bg-image('guarantee_3');
        &.invoice
          bg-image('invoice_3');
        &.special
          bg-image('special_3');
      .text
        display: table-cell;
        width: 56px;
        vertical-align: middle;
        border-1px(rgba(7, 17, 27, 0.1));
        font-size: 12px;
        font-weight: 200;
  .foods-wrapper
    flex: 1;
    .title
      height: 26px
      line-height: 26px
      padding-left: 14px
      border-left: 2px solid #d9dde1
      font-size: 12px
      color: rgb(147, 153, 159)
      background: #f3f5f7
    .food-item
      display: flex
      margin: 18px
      padding-bottom: 18px
      border-1px(rgba(7, 17, 27, .1))
      &:last-child
        border-none()
        // margin-bottom: 0
        padding-bottom: 0
      .icon
        flex: 0 0 57px
        margin-right: 10px
      .content
        flex: 1
        .name
          height: 14px
          line-height: 14px
          margin: 2px 0 8px 0
          font-size: 14px
          color: rgb(7, 17, 27)
        .desc, .extra
          line-height: 10px
          font-size: 10px
          color: rgb(147, 153, 159)
        .desc
          line-height: 12px
          margin-bottom 8px
        .extra
          .count
            margin-right: 12px
        .price
          font-weight: 700
          line-height: 24px
          .now
            margin-right: 8px
            font-size: 14px
            color: rgb(240, 20, 20)
          .old
            font-size: 10px
            text-decoration:  line-through
            color: rgb(147, 153, 159)
</style>
