<template>
  <div class="animation-element-wrapper" v-if="visibility">
    <div class="animation-element">
      <div style="height: 40px">
        <div class="left-bt text" @click.stop="handleCancel">取消</div>
        <div class="right-bt text" @click.stop="handleConfirm">确定</div>
        <view class="line"/>
      </div>
      <picker-view style="width: 100%; height: 300px;padding-top:35px" @change="change" indicator-style = "height: 50rpx;" :value="value">
        <picker-view-column >
        <view v-for="(item, index) in years" :key="index" class="item">
          {{item}}年
        </view>
        </picker-view-column>
        <picker-view-column >
          <view v-for="(item, index) in months" :key="index" class="item">
            {{item}}月
          </view>
        </picker-view-column>
        <picker-view-column >
          <view v-for="(item, index) in days" :key="index" class="item">
            {{item}}日
          </view>
        </picker-view-column>
      <picker-view-column>
          <view v-for="(item, index) in hours" :key="index" class="item">
            {{item}}时
          </view>
      </picker-view-column>
      <picker-view-column>
          <view v-for="(item, index) in minutes" :key="index" class="item">
            {{item}}分
          </view>
      </picker-view-column>
      <picker-view-column>
          <view v-for="(item, index) in seconds" :key="index" class="item">
            {{item}}秒
          </view>
      </picker-view-column>
    </picker-view>
    </div>
  </div>
</template>
<script>
const EVENTS_CHANGE = 'onChange'
const EVENTS_CONFIRM = 'onConfirm'
/* eslint-disable */
export default {
  data () {
    return {
      value: [],
      years: [],
      months: [],
      days: [],
      hours: [],
      minutes: [],
      seconds: [],
      year: '',
      month: '',
      day: '',
      hour: '',
      minute: '',
      second: '',
      visibility: false
    }
  },
  onLoad () {
    for (let i = new Date().getFullYear(); i < 2117; i++) {
      this.years.push(i)
    }
    for (let i = 1; i <= 12; i++) {
      i = this.zeroPadding(i)
      this.months.push(i)
    }
    for (let i = 1; i <= 31; i++) {
      i = this.zeroPadding(i);
      this.days.push(i)
    }
    for (let i = 0; i <= 23; i++) {
      i = this.zeroPadding(i);
      this.hours.push(i)
    }
    for (let i = 0; i <= 59; i++) {
      i = this.zeroPadding(i)
      this.minutes.push(i)
    }
    for (let i = 0; i <= 59; i++) {
      i = this.zeroPadding(i)
      this.seconds.push(i)
    }
    this.getNowDate(new Date())
    this.days = this.getDays(this.nowYear, this.nowMonth)
    this.value = [
      this.nowYear - this.nowYear, 
      this.nowMonth - 1, 
      this.nowDay - 1, 
      this.nowHour, 
      this.nowMinute, 
      this.nowSecond
    ]
    this.year = this.nowYear
    this.month = this.nowMonth
    this.day = this.zeroPadding(this.nowDay)
    this.hour = this.zeroPadding(this.nowHour)
    this.minute = this.zeroPadding(this.nowMinute)
    this.second = this.zeroPadding(this.nowSecond)
  },
  methods: {
    maskClick () {
      this.visibility = false
    },
    show () {
      this.visibility = true
    },
    hide () {
      this.visibility = false
    },
    handleCancel () {
      this.hide()
    },
    handleConfirm () {
      this.$emit(EVENTS_CONFIRM, `${this.year}-${this.month}-${this.day} ${this.hour}:${this.minute}:${this.second}`)
      this.hide()
    },
    getNowDate (date){
      this.nowYear = date.getFullYear()
      this.nowMonth = date.getMonth() + 1
      this.nowDay = date.getDate()
      this.nowHour = date.getHours()
      this.nowMinute = date.getMinutes()
      this.nowSecond = date.getSeconds()
    },
    getDays (year, month) {
      let dayCount = new Date(year, month, 0).getDate()
      let tempDays = []
      for (let i = 1; i <= dayCount; i++) {
        i = this.zeroPadding(i)
        tempDays.push(i)
      }
      return tempDays
    },
    zeroPadding (i) {
      return ('0' + i).slice(-2)
    },
    change ({mp}) {
      const val = mp.detail.value
      this.value = val
      this.days = this.getDays(val[0] + 2019, val[1] + 1)
      this.year = this.years[val[0]]
      this.month = this.months[val[1]]
      this.day = this.days[val[2]]
      this.hour = this.hours[val[3]]
      this.minute = this.minutes[val[4]]
      this.second = this.seconds[val[5]]
      this.$emit(EVENTS_CHANGE, `${this.year}-${this.month}-${this.day} ${this.hour}:${this.minute}:${this.second}`)
    }
  }
}
</script>
<style lang="stylus" scoped>
  .animation-element-wrapper 
    display: flex
    position: fixed
    left: 0
    top:0
    height: 100%
    width: 100%
    background-color: rgba(0, 0, 0, 0.6)
    .animation-element
      display: flex
      position: fixed
      width: 100%
      height: 470rpx
      bottom: 0
      background-color: rgba(255, 255, 255, 1)
  .item
    font-size: 14px
    line-height: 34px
  .text
    color: #999999
    display: inline-flex  
    position: fixed
    margin-top: 20rpx
    height: 50rpx
    text-align: center
    line-height: 50rpx
    font-size: 34rpx
    font-family: Arial, Helvetica, sans-serif
    z-index: 999
  .left-bt
    left: 30rpx
  .right-bt 
    right: 30rpx
  .line
    display: block
    position: fixed
    height: 1rpx
    width: 100%
    margin-top: 89rpx
    background-color: #eeeeee
</style>
