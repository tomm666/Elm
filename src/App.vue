<template>
  <div id="app">
  <vheader :seller="datas.seller"></vheader>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>

      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
   <!-- 把加载过来的组件缓存起来-->
    <keep-alive>
      <router-view :datas="datas" ref="abc"></router-view>
    </keep-alive>
  </div>
</template>

<script>
  import vheader from './components/mods/header.vue'
  import axios from 'axios'
  export default {
   data ()
  {
    return {
      datas: {
        // 商户信息
        seller: {},
        // 商品数据
        goods: {},
        // 评论
        ratings: {}
      }
    }
  },
  // 当组件加载完成 相当于 .ready
  mounted () {
    axios.get('./static/data.json').then(res => {
       this.datas.seller = res.data.seller
       this.datas.goods = res.data.goods
       this.datas.ratings = res.data.ratings
    //   console.log(this.$refs.abc)
      //  $nextTick等dom更新完之后再执行的函数
        this.$nextTick(() => {
          this.$refs.abc.scroll()
        })
  })
  },
  // 访问组件的时候执行
  activated () {
    // 有没有被缓存都会被执行这里
    console.log(123)
  },
  components: {
    vheader
  }
}
</script>

<!-- scoped  当写上 当前这个模板和当前模板下所有的子模板应用这个样式-->
<style scoped>
  .tab .tab-item a.router-link-active{
    color: red;
  }
</style>
