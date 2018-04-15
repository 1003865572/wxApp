<template>
  <div class="container">
    <ul class="meow">
      <li v-for="(item, index) in source" :key="index" @tap="playEvery(item)">
        {{item.icon}}
      </li>
    </ul>
    <div class="meow-btn">
      <div class="btn" @tap="playBackgroundAudio">
        <div class="bak-active"></div>
        <div class="bak"></div>
        <!-- <div class="text">
          {{status}}
        </div> -->
        <div v-if="status === 'play' || status === 'randomPlay'">
          <div class="text">播放中...</div>
        </div>
        <div v-if="status === 'playPause' || status === 'randomPlayPause'">
          <div class="text">暂停</div>
        </div>
        <div v-if="status === 'rady'">
          <div class="text">随机</div>
          <div class="text">喵叫</div>
        </div>
      </div>
      <p>按下这个按钮，你家的猫会来找你</p>
    </div>
    <ul class="meow">
      <li v-for="(item, index) in randomMp3List" :key="index" >
        {{item.icon}}
      </li>
    </ul>
  </div>
</template>

<script>
import card from '@/components/card'

const backgroundAudioManager = wx.getBackgroundAudioManager()

let mp3Source = [
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
]

export default {
  data () {
    return {
      status: 'rady',
      source: mp3Source,
      randomMp3List: [],
      userInfo: {}
    }
  },

  components: {
    card
  },
  methods: {
    /*
      status
        预备  rady
        随机播放  randomPlay
        随机暂停  randomPlayPause
        普通播放  play
        普通暂停  playPause

      如果第一次是随机
        随机
          第一次随机
          再一次暂停
          再一次随机
      如果第一次是播放
        点击暂停
        点击播放
        播放完毕清零
    */
    playBackgroundAudio () {
      if (this.status === 'rady') {
        if (this.randomMp3List.length === 0) {
          this.status = 'randomPlay'
          this.randomPlay()
        } else {
          this.status = 'play'
        }
      } else if (this.status === 'randomPlay') {
        this.status = 'randomPlayPause'
        backgroundAudioManager.pause()
      } else if (this.status === 'randomPlayPause') {
        this.status = 'randomPlay'
        backgroundAudioManager.play()
      } else if (this.status === 'play') {
        this.status = 'playPause'
        backgroundAudioManager.pause()
      } else if (this.status === 'playPause') {
        this.status = 'play'
        backgroundAudioManager.play()
      }
    },
    randomPlay () {
      let arr = Object.assign([], this.source)
      arr.sort((n1, n2) => {
        return Math.random() > 0.5 ? 1 : -1
      }).splice(0, 5)
      this.randomMp3List = arr
      this.playAudio()
    },
    playEvery (mp3Obj) {
      this.randomMp3List.unshift(mp3Obj)
      if (this.randomMp3List.length > 5) {
        this.randomMp3List.shift()
      }
      this.playAudio(mp3Obj)
    },
    bindViewTap () {
      const url = '../logs/main'
      wx.navigateTo({ url })
    },
    playAudio (mp3Obj) {
      if (this.randomMp3List.length === 0) {
        this.status = 'rady'
        return
      }
      if (!mp3Obj) {
        mp3Obj = this.randomMp3List[0]
      }
      backgroundAudioManager.src = mp3Obj.sourceUrl
      backgroundAudioManager.title = mp3Obj.text
      backgroundAudioManager.epname = mp3Obj.text
      backgroundAudioManager.singer = '喵叫'
      backgroundAudioManager.coverImgUrl = 'http://y.gtimg.cn/music/photo_new/T002R300x300M000003rsKF44GyaSk.jpg?max_age=2592000'
      backgroundAudioManager.play()
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
    backgroundAudioManager.onEnded((e) => {
      this.randomMp3List.shift()
      this.playAudio()
    })
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
  bottom: 110rpx;
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
