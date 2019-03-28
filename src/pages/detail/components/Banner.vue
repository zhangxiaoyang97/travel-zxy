<template>
  <div>
    <div class="banner" @click="handleBannerClick">
      <img class="banner-img" src="//img1.qunarzz.com/sight/p0/1505/74/749086a3617afc8c.water.jpg_600x330_367b25bf.jpg">
      <div class="banner-info">
        <div class="banner-title">富国海底世界(AAAA景区)</div>
        <div class="banner-number"><span class="iconfont banner-icon">&#xe692;</span> 39</div>
      </div>
    </div>
    <common-gallary :imgs="imgs" v-show="showGallary" @close="handelClose"></common-gallary>
  </div>
</template>

<script>
import CommonGallary from '../../../common/gallary/Gallary'
export default {
  name: 'DetailBanner',
  data () {
    return {
      showGallary: false,
      imgs: ['http://img1.qunarzz.com/sight/p0/1505/74/749086a3617afc8c.water.jpg_r_800x800_a76c1dd0.jpg', 'http://img1.qunarzz.com/sight/p0/1607/19/19c43fff95c719edc0.img.jpg_r_800x800_9f26b66d.jpg']
    }
  },
  components: {
    CommonGallary
  },
  methods: {
    handleBannerClick () {
      this.showGallary = true
    },
    handelClose () {
      this.showGallary = false
    }
  }
}
</script>

<style lang="stylus" scoped>
  .banner
    overflow: hidden
    height: 0
    position: relative
    color: #fff
    padding-bottom: 55%
    .banner-img
      width: 100%
    .banner-info
      display: flex
      position: absolute
      left: 0
      right: 0
      bottom: 0
      line-height: .6rem
      color: #fff
      background-image: linear-gradient(top, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.8))
      .banner-title
        flex: 1
        font-size: .32rem
        padding: 0 .2rem
      .banner-number
        height: .32rem
        line-height: .32rem
        padding: 0 .4rem
        margin-top: .14rem
        border-radius: .2rem
        font-size: .24rem
        background: rgba(0, 0, 0, 0.8)
        .banner-icon
          font-size: .24rem
</style>
