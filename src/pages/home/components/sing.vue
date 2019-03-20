<template>
  <div>
    <Table border ref="selection" :columns="columns4" :data="data1" @on-selection-change="selectCard"></Table>
    <!-- <Row>
      <Page
      class="page_conter" :class="this.flag?'show':'hidden'"
      :total="shopTotal"
      :page-size="pageSize"
      @on-page-size-change="sizeChange"
      @on-change="pageChange"
      :current="pageNo">
      </Page>
    </Row> -->
    <Row>
      <router-link :to="{path:'/sign', query:{cardId:this.cardId,couponIds:this.couponIds}}"><Button type="primary">保存</Button></router-link>
    </Row>
  </div>
</template>
<script>
import { couponList } from '@/api/data'
export default {
  data () {
    return {
      columns4: [
        {
          type: 'selection',
          width: 60,
          align: 'center'
        },
        {
          title: '有效期',
          key: 'days'
        },
        {
          title: '所属店铺名称',
          key: 'storeName'
        },
        {
          title: '优惠卷名称',
          key: 'description'
        }
      ],
      cardId: '',
      data1: [],
      flag: false,
      shopTotal: 0,
      pageNo: 1,
      pageSize: 20,
      couponIds: []
    }
  },
  mounted () {
    this.cardList()
    this.cardId = this.$route.query.cardId
  },
  methods: {
    sizeChange (pageSize) {
      this.pageSize = pageSize
      this.cardList()
    },
    pageChange (pageNo) {
      this.pageNo = pageNo
      this.cardList()
    },
    cardList () {
      couponList({
        pageNo: this.pageNo,
        pageSize: this.pageSize
      }).then(res => {
        if (res.resultCode === 1) {
          this.data1 = res.result
        }
      })
    },
    selectCard (selection) {
      this.couponIds = []
      for (let i = 0; i < selection.length; i++) {
        this.couponIds.push(selection[i].activityId)
      }
    }
  }
}
</script>
