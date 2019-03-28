<template>
  <div id="memberContainer">
    <i-form>
      <Form-item label="选择运营项目">
        <i-select v-model="currentStoreId" style="width:200px" @on-change="getRules" placeholder="请选择项目">
          <i-option v-for="(item,index) in storeListArr" :value="item.storeId" :key="index">{{ item.storeName }}</i-option>
        </i-select>
      </Form-item>
    </i-form>
    <section class="icon-list">
      <div v-for="(item,index) in cardList" :value="item.cardClassId" :key="index" class="icon-posi">
        <p>{{ item.cardClassName}}权益设置</p>
        <Row type="flex" justify="start" class="icon-row">
          <i-col span="3" v-for="(v, idx) in item.benefitList" :key="idx">
            <div class="icon-up">
              <Checkbox :label="v.enjoy" :key="v.navId" v-model="v.isEnjoy">
                <img :src="v.icon" class="icon-img">
                <i-input v-model="v.navName" class="icon-title" :id="v.navId"></i-input>
              </Checkbox>
            </div>
          </i-col>
        </Row>
        <i-form>
          <Form-item :label="item.cardClassName">
            <i-input placeholder="https://" v-model="item.router"></i-input>
          </Form-item>
          <Form-item label="" prop="file">
            <i-button @click="saveFile(item.cardClassId,item.cardClassName,item.rank,item.ruleBonus,item.image)" type="primary" class="save-btn" :id="item.cardClassId">保存</i-button>
            <Upload :action="base+'/ryh/config/upload'" ref="upload"
              :show-upload-list="false"
              :on-success="(value,file)=> handleSuccess(item.rank, value, file)"
              :format="['jpg','jpeg','png']">
              <Button>上传图片</Button>
            </Upload>
            <div v-if="imageShow">
              <img :src="item.image" style="max-width:480px;max-height:200px;">
            </div>
            <p v-if="item.rank === imageIndex">
              <img :src="imgSrc?imgSrc:''" alt name="imgSrc" style="max-width:480px;max-height:200px;">
            </p>
          </Form-item>
        </i-form>
      </div>
    </section>
    <section class="add-section clearfix" v-show="rule">
      <h5>新增权益设置</h5>
      <i-form>
        <Form-item class="mr10">
          <i-select style="width:160px" v-model="right0" :disabled="isDisabled">
            <i-option v-for="(item, index) in listSet" :value="item.order" :key="index">权益{{ item.text }}</i-option>
          </i-select>
        </Form-item>
        <Form-item class="grade-name mr10">
          <i-input placeholder="请输入权益名称" class="ipt" v-model="newInfo.navName"></i-input>
        </Form-item>
        <Form-item>
          <div><img :src="newInfo.icon"></div>
          <Upload :action="base+'/ryh/config/upload'"
          :show-upload-list="false"
          :on-success="uploadImgDone"
          :format="['jpg','jpeg','png']">
          <Button style="position:absolute;top:-57px;left:380px">上传图片</Button>
          </Upload>
        </Form-item>
        <Form-item style="text-align:center">
          <editor v-model="newInfo.comment" ref="editor" @on-change="handleChange"/>
        </Form-item>
      </i-form>
      <i-button @click="saveSet" type="primary" class="save-btn">保存</i-button>
    </section>
  </div>
</template>
<script>
import { getStoreList, getMemberRule, setMemberRule, list, addBenefit } from '@/api/data'
import { mapActions } from 'vuex'
import Editor from '_c/editor'
import baseURL from '_conf/url'
export default {
  data () {
    return {
      rule: false,
      storeListArr: [],
      currentStoreId: '',
      isDisabled: false,
      imageShow: true,
      iconCheckedArr: [],
      iconList: [],
      multichoice_default: [],
      answerInfo: [],
      listSet: [],
      idBefore: '',
      cardClassName: '', // 卡等级1:普卡2:银卡3:金卡4:黑卡
      cardList: [],
      cardList1: [],
      imageIndex: 0,
      right0: 0,
      imgData: '',
      avatar: '',
      imgSrc: '',
      base: '',
      content: '',
      userInfo: {},
      newInfo: {
        navName: '', // 权益描述
        Icon: '', // url地址
        comment: '' // 描述
      } // 新增权益信息
    }
  },
  components: {
    Editor
  },
  mounted () {
    this.userInfo = localStorage.getItem('userInfo') && JSON.parse(localStorage.getItem('userInfo'))
    this.base = baseURL
    this.storeList()
  },
  methods: {
    ...mapActions([
      'handleStore'
    ]),
    handleChange (html, text) {
      this.newInfo.comment = ''
      this.newInfo.comment = text
    },
    saveFile (e, name, rank, ruleBonus, image) {
      let data = {
        cardClassId: e, // 会员卡等级id
        cardClassName: name, // 卡名称
        rank: rank, // 卡等级1:普卡2:银卡3:金卡4:黑卡
        ruleBonus: ruleBonus, // 规则积分
        image: image, // 背景图
        router: this.cardList[rank - 1].router, // 路由地址
        benefitList: this.cardList[rank - 1].benefitList // 图标类表
      }
      setMemberRule(data).then(res => {
        if (res.resultCode === 1) {
          this.$Message.success('保存成功')
          this.storeList(this.$store.state.page.storeId)
        }
      })
    },
    // 选择运营项目
    storeList () {
      getStoreList({}).then(res => {
        this.storeListArr = res.result
        if (this.$store.state.page.storeId) {
          this.rule = true
          this.currentStoreId = this.$store.state.page.storeId
          this.getRules(this.$store.state.page.storeId)
        }
      })
    },
    // 获取会员卡列表
    getRules (e) {
      getMemberRule({
        storeId: e
      }).then(res => {
        if (res.resultCode === 1) {
          this.rule = true
          this.cardList = res.result
          this.cardList1 = res.result
          this.listset(e)
        }
      })
    },
    // 获取权益列表
    listset (e) {
      list({
        storeId: e
      }).then(res => {
        this.listSet = res.result
      })
    },
    handleSuccess (e, res, file) {
      this.imageIndex = e
      this.imgSrc = res.result
      this.cardList[e - 1].image = res.result
      this.imageShow = false
    },
    uploadImgDone (res, file) {
      this.newInfo.icon = res.result
      this.getRules(this.currentStoreId)
    },
    saveSet () {
      addBenefit({
        storeId: this.currentStoreId,
        sort: this.right0,
        navName: this.newInfo.navName,
        icon: this.newInfo.icon,
        comment: this.newInfo.comment,
        createUser: this.userInfo.id
      }).then(res => {
        if (res.resultCode === 1) {
          this.$Message.success('保存成功')
          this.getRules(this.currentStoreId)
        }
      })
    }
  }
}
</script>
<style lang="less">
.fl{
  float: left;
}
.clearfix:after{
  content: '';
  display:block;
  clear:both;
  height: 0;
}
.clearfix{
  zoom:1;
}
#memberContainer {
  .mt10{margin-top: 10px;}
  .mr10{margin-right: 10px;width: 18%;display: inline-block;}
  .ipt{
    width: 160px;
  }
  .tip{margin-left: 8px;color: #999;}
  .save-btn,.edit-btn{text-align:center}
  .ivu-upload {
    margin-left:30px;margin-top:-32px;
  }
  .icon-list{
    &>p{line-height: 30px;}
    p{margin: 13px 0;}
    .icon-row{
      .ivu-col{
        text-align:center;
        .icon-up{
          .icon-img{width:40px;height: 40px;margin-left: 10px;}
        }
        .icon-title{width:100px;margin-top:5px;margin-left: 10px;}
      }
    }
    .ivu-form-item{
      margin-top:20px;
      .ivu-form-item-content{
        .ivu-input-type{
          width:95%;
          .ivu-input{
            float:right;
          }
        }
      }
    }
  }
  .add-section{
    margin-top:10px;
    border-top:1px dashed #ccc;
    padding: 8px 0;
    h5{line-height: 30px;}
    .ivu-form-item-content{
      .avatar{
        div{
          img{
            max-width:480px;max-height:200px;
          }
        }
      }
      .ivu-select{
        .ivu-select-dropdown{
          z-index: 99999;
        }
      }
    }
  }
}
</style>
