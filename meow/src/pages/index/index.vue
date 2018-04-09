<template>
  <div class="container" @click="clickHandle('test click', $event)">
    <ul class="meow">
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
    <ul class="meow">
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
    <div class="meow-btn">
      <div class="btn">
        <div class="bak-active"></div>
        <div class="bak"></div>
        <div class="text">随机</div>
        <div class="text">喵叫</div>
      </div>
      按下这个按钮，小猫会来找你
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

export default {
  data () {
    return {
      motto: 'Hello World',
      userInfo: {}
    }
  },

  components: {
    card
  },

  methods: {
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
  flex: 2;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
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
  transition: all .2s ease;
}
.meow-btn:active .bak-active{
  transform: scale(1.1);
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
  display: flex;
  flex-direction: row;
  align-items: center;
}
.meow li{
  width: 100rpx;
  height: 100rpx;
  margin: 10rpx;
  border: 1px solid #fff;
  border-radius: 15rpx;
}
.container{
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  background: #00BBB2;
}
.counter {
  display: inline-block;
  margin: 10px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
}
</style>
