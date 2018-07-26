<template>
  <div ref="ratingsWrapper" class="ratingsWrapper">
   <div class="ratings-content">
     <div class="info">
       <div class="mark">
         <div class="num">{{datas.seller.score}}</div>
         <div class="text">综合评分</div>
         <div class="contrast">高于周边商家{{datas.seller.rankRate}}%</div>
       </div>
       <div class="stars">
         <div class="serviceScore">
           <span class="text">服务态度</span>
           <star :size="36" :score="datas.seller.serviceScore"></star>
           <span class="num">{{datas.seller.serviceScore}}</span>
         </div>
         <div class="foodScore">
           <span class="text">商品评分</span>
           <star :size="36" :score="datas.seller.serviceScore"></star>
           <span class="num">{{datas.seller.serviceScore}}</span>
         </div>
       </div>
     </div>
     <div class="divider"></div>
     <div class="evaluation">
       <div class="classify">
         <span class="item"  @click="evel(item)"
             :class="{'active': item.active, 'bad': index===2, 'badActive': item.active&&index===2 }"
               v-for="(item,index) in classify">
           {{item.name}}
           <span class="count">{{item.count}}</span>
         </span>
       </div>
       <div class="switch" @click="show = !show">
         <span class="icon-check_circle" :class="{'on': show}"></span>
         <span class="text">只看有内容评价</span>
       </div>
       <div class="evel-list">
         <ul>
           <li class="evel" v-for="item in comments">
             <div class="avatar">
               <img :src="item.avatar" width="28" height="28" alt="头像"/>
             </div>
             <div class="content">
               <div class="user">
                 <span class="name">{{item.username}}</span>
                 <span class="rateTime">{{item.rateTime}}</span>
               </div>
               <div class="star-wrapper">
                 <star :size="24" :score="item.score"></star>
                 <span class="deliveryTime">{{item.deliveryTime}}分钟送达</span>
               </div>
               <div class="text">{{item.text}}</div>
               <div class="recommend">
                 <span class="icon" :class="item.rateType?'icon-thumb_down':'icon-thumb_up' "></span>
                 <span class="dish" v-for="dish in item.recommend">{{dish}}</span>
               </div>
             </div>
           </li>
         </ul>
       </div>
     </div>
   </div>
  </div>
  </template>
<script>
  import axios from 'axios'
  import Scroll from 'better-scroll'
  import star from './mods/star.vue'
  export default {
    props: ['datas'],
    data () {
    return {
      ratings: [],
      classify: [
        {name: '全部', active: true, count: 0, comment: []},
        {name: '全部', active: false, count: 0, comment: []},
        {name: '全部', active: false, count: 0, comment: []}
      ],
      show: true,
      // 保存每次点击的类型下的评论
      comments: []
      }
    },
    mounted () {
    axios.get('/static/data.json').then(res => {
      //  注册了一个$nextTick => 更新dom => 触发了$nextTick => new 滚动 => 计算属性里执行 => 更新dom =>
      //   this.$nextTick(() => {
     //    this.$nextTick(() => {
     //     this.sc = new Scroll(this.$refs['ratingsWrapper'], {
    //        click: true
    //   })
   // })
   //  })
      this.ratings = res.data.ratings
      this.comments = res.data.ratings
      this.ratings.forEach(val => {
        this.classify[0].count++
        this.classify[0].comment.push(val)
        if (val.rateType) {
          this.classify[2].count++
          this.classify[2].comment.push(val)
        } else {
          this.classify[1].count++
          this.classify[1].comment.push(val)
        }
      })
      // 这里是刚刚把评论归类好 => 注册了一个$nextTick => 计算属性里执行 => 更新dom
      // => 触发了$nextTick => new 滚动插件
      this.$nextTick(() => {
        this.sc = new Scroll(this.$refs['ratingsWrapper'], {
        click: true
      })
      })
    })
  },
  methods: {
    evel (item) {
        this.classify.forEach(val => {
          val.active = false
        })
        this.comments = item.comment
        item.active = true
    }
  },
  computed: {
    comment () {
     if (this.show) {
       // 只看有内容评价
       // 把筛选出来的数据保存起来
       let arr = []
       this.comments.forEach(val => {
         if (val.text.length > 0) {
          arr.push(val)
          }
       })
      return arr
     } else {
      // 看全部评价
      return this.comments
    }
    }
  },
    components: {
      star
    }
  }
</script>

