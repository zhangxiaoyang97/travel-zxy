<template>
  <div>
    <div class="recommend-title">热销推荐</div>
    <ul>
      <li class="item border-bottom" v-for="item of recommendList" :key="item.id">
        <img class="item-img" :src="item.imgUrl" />
        <div class="item-info">
          <p class="item-title">{{item.title}}</p>
          <p class="item-desc">{{item.desc}}</p>
          <button class="item-button">查看详情</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HomeRecommend',
  data () {
    return {
      recommendList: [{
        id: '0001',
        imgUrl: 'http://img1.qunarzz.com/sight/p0/1812/c1/c156e13ddd316b2ba3.img.jpg_200x200_4dcee529.jpg',
        title: '北京活的3D博物馆',
        desc: '有八座古寺而得名，香山外另一爬山绝佳地有八座古寺而得名，香山外另一爬山绝佳地'
      }, {
        id: '0002',
        imgUrl: 'http://img1.qunarzz.com/sight/p0/201307/18/73703f0304835b9ac8d65eac.jpg_200x200_12107416.jpg',
        title: '北京活的3D博物馆',
        desc: '有八座古寺而得名，香山外另一爬山绝佳地有八座古寺而得名，香山外另一爬山绝佳地'
      }, {
        id: '0003',
        imgUrl: 'http://img1.qunarzz.com/sight/p0/1505/f5/f5f45e1a83537bcb.water.jpg_200x200_444cc7e8.jpg',
        title: '北京活的3D博物馆',
        desc: '有八座古寺而得名，香山外另一爬山绝佳地有八座古寺而得名，香山外另一爬山绝佳地'
      }, {
        id: '0004',
        imgUrl: 'http://img1.qunarzz.com/sight/p0/1703/88/88b6e81d8506213a3.img.jpg_200x200_6d009208.jpg',
        title: '北京活的3D博物馆',
        desc: '有八座古寺而得名，香山外另一爬山绝佳地有八座古寺而得名，香山外另一爬山绝佳地'
      }]
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import '~@/assets/styles/mixins.styl'
  .recommend-title
    margin-top: .2rem
    line-height: .44rem
    background: #eee
    text-indent: .2rem
  .item
    overflow: hidden
    display: flex
    height: 1.9rem
    .item-img
      width: 1.7rem
      height: 1.7rem
      padding: .1rem
    .item-info
      flex: 1
      padding: .1rem
      min-width: 0
      .item-title
        line-height: .54rem
        font-size: .32rem
        ellipsis()
      .item-desc
        overflow: hidden
        line-height: .4rem
        height: .4rem
        ellipsis()
      .item-button
        line-height: .44rem
        margin-top: .2rem
        background: #ff9300
        padding: 0 .2rem
        color: #fff
        border-radius: .06rem
</style>
