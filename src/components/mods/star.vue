<template>
    <div class="star" :class="'star-' + size">
      <span class="star-item" :class="item" v-for="item in yimao"></span>
    </div>
</template>

<script>
  export default  {
    props: ['score', 'size'],
    computed: {
      yimao () {
        let arr = []
        // 获取整数部分 向下取整
        let taiyang = Math.floor(this.score)
        //  获取小数部分
        let xiaoshu = (this.score - taiyang) >= 0.5
            for (let i = 0;i < taiyang; i++){
              arr.push('on')
            }
            if (xiaoshu) {
              arr.push('half')
            }
            while (arr.length < 5){
              arr.push('off')
            }
            return arr
      }
    }
  }
</script>
