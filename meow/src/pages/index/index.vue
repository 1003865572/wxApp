<template>
  <div class="container">
    <ul class="meow">
      <li v-for="(item, index) in source" :key="index" >
        {{item.icon}}
      </li>
    </ul>
    <div class="meow-btn">
      <div class="btn" @tap="playBackgroundAudio">
        <div class="bak-active"></div>
        <div class="bak"></div>
        <div class="text">随机</div>
        <div class="text">喵叫</div>
      </div>
      <p>按下这个按钮，你家的猫会来找你</p>
    </div>
    <ul class="meow">
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </div>
</template>

<script>
import card from '@/components/card'

const backgroundAudioManager = wx.getBackgroundAudioManager()

export default {
  data () {
    return {
      motto: 'Hello World',
      source: [
        {icon: '1', text: '打呼噜', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8TvyUstthTP.mp3'},
        {icon: '2', text: '缠你', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8WRHBqcpjtE.mp3'},
        {icon: '3', text: '大猫叫', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8TUqIVfjCqo.mp3'},
        {icon: '4', text: '小猫叫', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8xuAgRMkoZs.mp3'},
        {icon: '5', text: '老猫叫', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8wbcXtfQHKM.mp3'},
        {icon: '6', text: '喵叫', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8HeQzppsVNH.mp3'},
        {icon: '7', text: '撒娇', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8yfZMhAUNyD.mp3'},
        {icon: '8', text: '小猫叫声', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8gFvkVmahRv.mp3'},
        {icon: '9', text: '要食物', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8bpHbEjnbxZ.mp3'},
        {icon: '10', text: '野外叫', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8YTnzDtcJkN.mp3'},
        {icon: '11', text: '半大猫叫', sourceUrl: 'https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv7IXipGDSssC.mp3'}
      ],
      userInfo: {}
    }
  },

  components: {
    card
  },

  methods: {
    playBackgroundAudio () {
      if (!backgroundAudioManager.src) {
        backgroundAudioManager.title = '喵叫'
        backgroundAudioManager.epname = '喵叫'
        backgroundAudioManager.singer = ''
        backgroundAudioManager.src = `https://cloud-minapp-13037.cloud.ifanrusercontent.com/1f5vv8gFvkVmahRv.mp3`
      }
      if (!backgroundAudioManager.paused) {
        backgroundAudioManager.pause()
      } else {
        backgroundAudioManager.play()
      }
    },
    bindViewTap () {
      const url = '../logs/main'
      wx.navigateTo({ url })
    },
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        }
      })
    },
    clickHandle (msg, ev) {
      console.log('clickHandle:', msg, ev)
    }
  },

  created () {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
  }
}
</script>

<style scoped>
.meow-btn{
  flex: 3;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}
.meow-btn p{
  position: absolute;
  width: 100vw;
  text-align: center;
  color: #fff;
  bottom: 20rpx;
  font-size: 26rpx;
}

.bak-active, .bak{
  position: absolute;
  top: 0;
  left: 0;
  width: 220rpx;
  height: 220rpx;
  z-index: 3;
  background: #fff;
  border-radius: 15rpx;
}
.bak-active{
  background: #00DDD6;
  z-index: 1;
  width: 220rpx;
  height: 220rpx;
  transition: all .4s ease;
}
.meow-btn:active .bak-active{
  transform: scale(1.25);
}
.meow-btn .text{
  position: relative;
  z-index: 4;
}
.meow-btn .btn{
  position: relative;
  width: 220rpx;
  height: 220rpx;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #008C78;
  border-radius: 35rpx;
}
.meow{
  flex: 1;
  text-align: center;
}
.meow li{
  width: 100rpx;
  height: 100rpx;
  display: inline-block;
  margin: 10rpx;
  background: #21CBBF;
  border-radius: 15rpx;
}
.container{
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  overflow: hidden;
  background: #00A798;
  padding: 100rpx 0;
}
.counter {
  display: inline-block;
  margin: 10px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
}
</style>
