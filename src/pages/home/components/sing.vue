<template>
  <div>
    <i-form>
      <Form-item label="选择运营项目">
        <i-select v-model="currentStoreId" style="width:200px" @on-change="storeChange" placeholder="请选择项目">
            <i-option v-for="(item,index) in cityList" :value="item.storeId" :key="index">{{ item.storeName }}</i-option>
        </i-select>
      </Form-item>
    </i-form>

    <!-- <section>
      <div v-for="(item, i) in adList" :key="i" class="advImg">{{item.sort}}
        <img :src="item.image">
        <Input v-model="item.path" size="small"/>
      </div>
      <adver :data="adInfo" :openAd="openAd" @getImgData="getImgData" @changeStatus="changeAdStatus" @getImgLink="getImgLink" ref="adver">
        <span>广告位设置</span>
      </adver>
      <i-button class="add-btn" type="primary" @click="saveAdSet">保存</i-button>
    </section>
    <div id="coupons" v-show="rule">
      <card>
        <RadioGroup v-model="vertical" vertical>
          <Radio label="rule1">
            <span>签到规则一</span>
          </Radio>
        </RadioGroup>
        <Row class="time-set">
          <Col span="4" class="data-title">
             启用时间段
          </Col>
          <Col span="8">
            开始时间:
            <date-picker type="datetime" v-model="startTime1" placeholder="请选择开始时间"></date-picker>
          </Col>
          <Col span="8">
            结束时间:
            <date-picker type="datetime" v-model="endTime1" placeholder="请选择结束时间"></date-picker>
          </Col>
          <Col span="4" class="data-title">
            <Checkbox v-model="single">长期</Checkbox>
          </Col>
        </Row>
        <Row>
          <Col span="8" offset="0" class="data-title">
            签到积分值
            <Input v-model="value3" size="small" clearable style="width:100px;"  />
          </Col>
        </Row>
        <Row>
          <Col span="8" offset="0">
            <span style="margin:0 20px;">随机分值</span>
            <RadioGroup v-model="random" randow>
              <Radio label="true">
                <span>是</span>
              </Radio>
              <Radio label="false">
                <span>否</span>
              </Radio>
            </RadioGroup>
          </Col>
        </Row>
        <div class="number" v-show="this.random === 'true'">
          <RadioGroup v-model="number">
            <Radio label="10">
              <span>10分以内</span>
            </Radio>
            <Radio label="20">
              <span>20分以内</span>
            </Radio>
            <Radio label="30">
              <span>30分以内</span>
            </Radio>
            <Radio label="40">
              <span>40分以内</span>
            </Radio>
            <Radio label="50">
              <span>50分以内</span>
            </Radio>
          </RadioGroup>
        </div>
        <div class="number">
          <Checkbox v-model="card">连续签到送卡劵</Checkbox>
          <Select v-model="day" clearable style="width:100px;" size="small">
            <Option v-for="item in dayList" :value="item.value" :key="item.value">{{ item.label }}</Option>
          </Select>
          <router-link :to="{path:'/card', query:{cardId:1}}"><span class="choose">选择卡劵</span></router-link>
        </div>
        <div style="text-align:center;">
          <Button type="primary" @click="surePlan">确认使用该方案</Button>
        </div>
      </card>

      <card style="margin-top:20px;">
        <RadioGroup v-model="vertical" vertical>
          <Radio label="rule2">
            <span>签到规则二</span>
          </Radio>
        </RadioGroup>
        <Row class="time-set">
          <Col span="4" class="data-title">
             启用时间段
          </Col>
          <Col span="8"> 开始时间:
            <date-picker type="datetime" v-model="startTime" placeholder="请选择开始时间"></date-picker>
          </Col>
          <Col span="8"> 结束时间:
            <date-picker type="datetime" v-model="endTime" placeholder="请选择结束时间"></date-picker>
          </Col>
          <Col span="4" class="data-title">
            <Checkbox v-model="single2">长期</Checkbox>
          </Col>
        </Row>

        <Row>
          <Col class="data-title" offset="0">
            签到积分起始值
            <Select v-model="integral2" clearable style="width:80px;margin-left:10px;" size="small">
              <Option v-for="item in cityList2" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
          </Col>
        </Row>
        <Row>
          <Col class="data-title" offset="0">
            连续签到周期
            <Select v-model="continuous" clearable style="width:80px;margin-left:10px;" size="small">
              <Option v-for="item in getList" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
          </Col>
        </Row>

        <Row>
          <Col span="10" class="data-title">
            连续签到倍率
            <RadioGroup v-model="number1">
              <Radio label="2">
                <span>次日*2</span>
              </Radio>
              <Radio label="3">
                <span>次日*3</span>
              </Radio>
              <Radio label="5">
                <span>次日*5</span>
              </Radio>
            </RadioGroup>
          </Col>
          <Col span="2" style="margin-top:5px;">
            或
          </Col>
          <Col span="8" style="margin-top:3px;">
            次日增加
            <Input v-model="define" @on-change="addInput" size="small" clearable style="width:150px;" />
          </Col>
        </Row>

        <div class="number">
          <Checkbox v-model="card1">连续签到送卡劵</Checkbox>
          <Select v-model="day1" clearable style="width:100px;" size="small">
            <Option v-for="item in dayList1" :value="item.value" :key="item.value">{{ item.label }}</Option>
          </Select>
          <router-link :to="{ path:'/card',query: {cardId:2}}"><span class="choose">选择卡劵</span></router-link>
        </div>
        <div style="text-align:center;">
          <Button type="primary" @click="surePlan1">确认使用该方案</Button>
        </div>
      </card>
    </div> -->
  </div>
</template>
<script>
import { getStoreList, allSingRule, addRule, addRule1, navAdd, signLayout, disFloor } from '@/api/data'
import { mapActions } from 'vuex'
import adver from '../components/ad/ad'
import uploadAvatar from '../index-cms/upload'
export default {
  name: 'sign',
  components: {
    uploadAvatar,
    adver
  },
  data () {
    return {
      rule: false,
      currentStoreId: '',
      cityList: [],
      vertical: '',
      single: false,
      single2: false,
      random: 'false',
      card: true,
      card1: true,
      integral: '',
      integral2: 0,
      continuous: 0,
      add: '',
      define: '',
      day: 0,
      day1: 0,
      value3: '',
      cardTitle: '',
      cardTitle1: '',
      number: '',
      number1: '',
      ruleId1: '',
      ruleId2: '',
      cityList2: [
        {
          value: 1,
          label: '1分'
        },
        {
          value: 2,
          label: '2分'
        },
        {
          value: 3,
          label: '3分'
        },
        {
          value: 4,
          label: '4分'
        },
        {
          value: 5,
          label: '5分'
        },
        {
          value: 6,
          label: '6分'
        },
        {
          value: 7,
          label: '7分'
        },
        {
          value: 8,
          label: '8分'
        },
        {
          value: 9,
          label: '9分'
        },
        {
          value: 10,
          label: '10分'
        }
      ],
      getList: [
        {
          value: 3,
          label: '3天'
        },
        {
          value: 5,
          label: '5天'
        },
        {
          value: 7,
          label: '7天'
        },
        {
          value: 10,
          label: '10天'
        }
      ],
      dayList: [
        {
          value: 3,
          label: '3'
        },
        {
          value: 5,
          label: '5'
        },
        {
          value: 7,
          label: '7'
        },
        {
          value: 10,
          label: '10'
        }
      ],
      dayList1: [
        {
          value: 3,
          label: '3'
        },
        {
          value: 5,
          label: '5'
        },
        {
          value: 7,
          label: '7'
        },
        {
          value: 10,
          label: '10'
        }
      ],
      adList: [],
      adInfo: {
        floorId: '',
        url: '',
        src: '',
        status: false
      },
      openAd: false,
      userInfo: {},
      parentMsg: '', // 选择完项目获取到的数据
      startTime: '',
      endTime: '',
      startTime1: '',
      endTime1: '',
      storeId: '',
      ruleType: '',
      dayBonus: '',
      randomRangeEnd: '',
      periodDays: '',
      continueDays: 0,
      continueDays1: 0,
      additionalBonus: 0,
      couponIds: [],
      additionalBonus1: 0,
      couponIds1: [],
      cardId: this.$route.query.cardId
    }
  },
  mounted () {
    console.log("aaa")
    this.storeList()
    // this.userInfo = localStorage.getItem('userInfo') && JSON.parse(localStorage.getItem('userInfo'))
  },
  methods: {
    ...mapActions([
      'handleStore'
    ]),
    /*consoleList () {
      for (let i = 0; i < this.parentMsg.length; i++) {
        if (this.parentMsg[i].ruleType === 1 || this.parentMsg[i].ruleType === 2) {
          this.single = this.parentMsg[i].isPermanent ? this.parentMsg[i].isPermanent : false
          this.startTime1 = this.parentMsg[i].startTime === null ? '' : this.parentMsg[i].startTime
          this.endTime1 = this.parentMsg[i].endTime === null ? '' : this.parentMsg[i].endTime
          this.value3 = this.parentMsg[i].dayBonus ? this.parentMsg[i].dayBonus : ''
          this.number = this.parentMsg[i].randomRangeEnd === null ? '' : this.parentMsg[i].randomRangeEnd.toString()
          if (this.parentMsg[i].ruleType === 1) {
            this.random = 'false'
          } else {
            this.random = 'true'
          }
          if (this.parentMsg[i].isValid === true) {
            this.vertical = 'rule1'
            this.ruleId1 = this.parentMsg[i].ruleId
          }
          if (this.parentMsg[i].rewards.length !== 0) {
            this.day = this.parentMsg[i].rewards[0].continueDays
            this.additionalBonus = this.parentMsg[i].rewards[0].additionalBonus
            this.couponIds = this.parentMsg[i].rewards[0].couponIds
          }
        } else {
          if (this.parentMsg[i].isValid === true) {
            this.vertical = 'rule2'
            this.ruleId2 = this.parentMsg[i].ruleId
          }
          this.integral2 = this.parentMsg[i].bonusStart ? this.parentMsg[i].bonusStart : ''
          this.startTime = this.parentMsg[i].startTime === null ? '' : this.parentMsg[i].startTime
          this.endTime = this.parentMsg[i].endTime === null ? '' : this.parentMsg[i].endTime
          this.continuous = this.parentMsg[i].periodDays ? this.parentMsg[i].periodDays : ''
          this.single2 = this.parentMsg[i].isPermanent ? this.parentMsg[i].isPermanent : false
          this.number1 = this.parentMsg[i].multiple ? this.parentMsg[i].multiple.toString() : ''
          this.define = this.parentMsg[i].bonusIncrease ? this.parentMsg[i].bonusIncrease.toString() : ''
          if (this.parentMsg[i].rewards.length !== 0) {
            this.day1 = this.parentMsg[i].rewards[0].continueDays
            this.additionalBonus1 = this.parentMsg[i].rewards[0].additionalBonus
            this.couponIds1 = this.parentMsg[i].rewards[0].couponIds
          }
        }
      }
    },
    signLayout () {
      signLayout({
        storeId: this.currentStoreId
      }).then(res => {
        if (res.resultCode === 1) {
          this.adInfo.floorId = res.result.floorId
          if (res.result.status === '10A') {
            this.adInfo.status = true
          } else {
            this.adInfo.status = false
          }
          this.adList = res.result.nav
        } else {
          alert(res.resultMsg)
        }
      })
    },
    changeDate (dateA) {
      if (dateA !== '') {
        let dateee = new Date(dateA).toJSON()
        let date = new Date(+new Date(dateee) + 8 * 3600 * 1000).toISOString().replace(/T/g, ' ').replace(/\.[\d]{3}Z/, '')
        return date
      }
    },
    changeAdStatus (v) {
      this.openAd = v
      this.adInfo.status = v
      disFloor({
        floorId: this.adInfo.floorId,
        status: this.adInfo.status
      }).then(res => {
        console.log(res)
      })
    },
    saveAdSet () {
      navAdd({
        'floorId': this.adInfo.floorId,
        'image': this.adInfo.src || '',
        'path': this.adInfo.url || '',
        'createUser': this.userInfo.id
      }).then(res => {
        if (res.resultCode === 1) {
          this.$Message.success('保存成功')
          this.signLayout(this.$store.state.page.storeId)
        }
      })
    },
    getImgData (img) {
      this.adInfo.src = img
    },
    getImgLink (v) {
      this.adInfo.url = v
    },
    surePlan () {
      this.cardId = this.$route.query.cardId
      if (this.storeId === '') {
        alert('请选择运营项目')
        return false
      } else {
        if (this.vertical !== 'rule1') {
          alert('请勾选签到规则1')
          return false
        } else {
          if (this.random === 'true') {
            if (this.number === '') {
              alert('请选择随机积分的数值')
              return false
            } else {
              this.ruleType = 2
            }
          } else {
            if (this.value3 === '') {
              alert('请填写签到积分值')
              return false
            } else {
              this.ruleType = 1
            }
          }
          if (this.single === true) {
            if (this.startTime1 !== '' && this.endTime1 !== '') {
              alert('长期和时间不能同时选择')
              return false
            }
          } else {
            if (this.startTime1 !== '' && this.endTime1 === '') {
              alert('请选择结束时间')
              return false
            } else if (this.endTime1 !== '' && this.startTime1 === '') {
              alert('请选择开始时间')
              return false
            }
          }
        }
        if (this.cardId === 1) {
          this.couponIds = this.$route.query.couponIds
        }
        addRule({
          storeId: this.storeId,
          ruleType: this.ruleType,
          ruleId: this.ruleId1,
          dayBonus: this.value3,
          randomRangeStart: 0,
          randomRangeEnd: this.number,
          startTime: this.changeDate(this.startTime1),
          endTime: this.changeDate(this.endTime1),
          isPermanent: this.single,
          userId: this.userInfo.id,
          rewards: [
            {continueDays: this.day, additionalBonus: this.additionalBonus, couponIds: this.couponIds}
          ]
        }).then(res => {
          if (res.resultCode === 1) {
            this.$Message.success('保存成功')
            this.signLayout(this.$store.state.page.storeId)
          }
        })
      }
    },
    surePlan1 () {
      this.cardId = this.$route.query.cardId
      if (this.storeId === '') {
        alert('请选择运营项目')
        return false
      } else {
        if (this.vertical !== 'rule2') {
          alert('请勾选签到规则2')
          return false
        } else {
          this.ruleType = 3
          if (this.integral2 === null || this.integral2 === '') {
            alert('累计签到积分起始值不能为空')
            return false
          }
          if (this.single2 === true) {
            if (this.startTime !== '' || this.endTime !== '') {
              alert('长期和时间不能同时选择')
              return false
            }
          } else {
            if (this.startTime === '' || this.endTime === '') {
              alert('长期和时间必须选择一个')
              return false
            }
          }
          if (this.number1 !== '' && this.define !== '') {
            alert('签到倍率和次日增加只能选择一个')
            return false
          }
        }
        if (this.cardId === 2) {
          this.couponIds1 = this.$route.query.couponIds
        }
        addRule1({
          storeId: this.storeId,
          ruleType: this.ruleType,
          ruleId: this.ruleId2,
          periodDays: this.continuous,
          bonusStart: this.integral2,
          multiple: this.number1,
          bonusIncrease: this.define,
          startTime: this.changeDate(this.startTime),
          endTime: this.changeDate(this.endTime),
          isPermanent: this.single2,
          userId: this.userInfo.id,
          rewards: [
            {continueDays: this.day1, additionalBonus: this.additionalBonus1, couponIds: this.couponIds1}
          ]
        }).then(res => {
          if (res.resultCode === 1) {
            this.$Message.success('保存成功')
            this.signLayout(this.$store.state.page.storeId)
          }
        })
      }
    },
    addInput () {
      this.number1 = ''
    },*/
    /* 项目列表 */
    storeList () {
      console.log("bbbb")
      getStoreList({}).then(res => {
        this.cityList = res.result
        if (this.$store.state.page.storeId) {
          this.currentStoreId = this.$store.state.page.storeId
          this.rule = true
          this.storeChange(this.$store.state.page.storeId)
        }
        console.log(this.currentStoreId)
      })
    },
    /* 选择运营项目 */
    storeChange (e) {
      console.log(e)
      allSingRule({
        storeId: e
      }).then(res => {
        /*this.rule = true
        this.parentMsg = res.result
        this.consoleList(e)
        this.signLayout(e)*/
      })
    }
  }
}
</script>
<style lang="less" scoped>
.data-title {
  padding-top:8px;
  padding-left:20px;
}
.time-set {
  margin:15px 0;
}
.datat {
  margin-left:35px;
}
#coupons .ivu-row {
  margin-top:20px;
}
.number {
  margin: 20px;
}
.choose {
  border:1px solid #ccc;
  border-radius:3px;
  padding:4px 5px;
  margin:2px 10px;
  cursor:pointer;
}
.add-btn{
  margin-left:20px;
  margin-bottom:10px;
}
.advImg img {
  max-width:480px;max-height:200px
}

</style>
