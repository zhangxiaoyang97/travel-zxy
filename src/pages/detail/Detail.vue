<template>
  <div>
    <detail-banner></detail-banner>
    <detail-header></detail-header>
    <div class="content">
      <detail-list :list='listA'></detail-list>
    </div>
  </div>
</template>

<script>
import DetailBanner from './components/Banner'
import DetailHeader from './components/Header'
import DetailList from './components/List'
export default {
  name: 'Detail',
  components: {
    DetailBanner,
    DetailHeader,
    DetailList
  },
  data () {
    return {
      listA: [{
        title: '001',
        children: [{
          title: '成人三管联票',
          children: [{
            title: '001三管联票3'
          }]
        }, {
          title: '成人五管联票'
        }]
      }, {
        title: '002',
        children: [{
          title: '002三管联票'
        }, {
          title: '002五管联票'
        }]
      }, {
        title: '003',
        children: [{
          title: '003三管联票'
        }, {
          title: '003五管联票'
        }]
      }]
    }
  }
}
</script>

<style lang="stylus" scoped>
  .content {
    height: 50rem
  }
</style>
