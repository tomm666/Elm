<template>
  <div ref="sellerwrapper" class="seller-wrapper">
      <div class="seller-content">
        <div class="info">
          <div class="title">
            <div class="text">{{datas.seller.name}}</div>
            <div class="star-wrapper">
              <star :size="36" :score="datas.seller.score"></star>
              <div class="rate-count">{{datas.seller.ratingCount}}</div>
              <div class="sell-count">{{datas.seller.sellCount}}</div>
            </div>
            <div @click="show = !show" class="collect">
              <span class="icon-favorite" :class="{'active': show}"></span>
              <span class="text">{{show? '已收藏': '收藏'}}</span>
            </div>
            <div class="remark">
              <div class="block">
                <h2>起送价</h2>
                <div class="content">
                  <span class="num">{{datas.seller.minPrice}}</span>
                </div>
              </div>
              <div class="block">
                <h2>商家配送</h2>
                <div class="content">
                  <span class="num">{{datas.seller.deliveryPrice}}</span>
                </div>
              </div>
              <div class="block">
                <h2>平均配送时间</h2>
                <div class="content">
                  <span class="num">{{datas.seller.deliveryTime}}</span>
                </div>
              </div>
            </div>
            <div class="activities">
              <div class="bulletin">
                <h1>公告与活动</h1>
                <div class="content">{{datas.seller.bulletin}}</div>
              </div>
            </div>
            <div class="supports">
              <ul>
                <li class="item" v-for="item in datas.seller.supports">
                  <span class="iconMap" :class="icon[item.type]"></span>
                  <span class="text">{{item.description}}</span>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>
<script>
  import icon from './mods/icon'
  import Scroll from 'better-scroll'
  import star from './mods/star'
  export default {
    props: ['datas'],
    data () {
      return {
        show: false,
        icon: icon
      }
  },
  created () {
    // 等dom 更新所执行的方法
    this.$nextTick(() => {
      /* eslint-disable no-new */
      new Scroll(this.$refs['sellerwrapper'])
    })
  },
  // 组件初始化之后
   //  mounted () {
    // 可以直接使用滚动插件 没有写计算属性 方法 重新更新dom
    /* eslint-disable no-new */
 //   new Scroll(this.$refs['sellerwrapper'])
 // }
  components: {
    star
  }
  }
</script>
