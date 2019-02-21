```javascript
<template>
  <view class="video-component"
        style="width: {{videoWidth}}; height: {{videoHeight}}">
    <video class="el-video"
            id="el-home-video"
            src="{{src}}"
            bindplay="bindplay"
            bindpause="bindpause"
            bindwaiting="bindwaiting"
            binderror="binderror"
            custom-cache="{{false}}"
            objectFit="{{objectFit}}"
            muted="{{muted}}"
            autoplay="{{autoplay}}"
            loop="{{loop}}"
            show-center-play-btn="{{showCenterPlayBtn}}"
            controls="{{controls}}">

      <!-- <solt name="video"></solt> -->
    </video>

    <canvas class='video-up-canvas'
            style="width: {{videoWidth}}; height: {{videoHeight}}"
            canvas-id="video-up-canvas">
      <solt></solt>
    </canvas>
  </view>
</template>

<script>
import wepy from 'wepy'

export default class videoComponent extends wepy.component {
  props = {
    videoWidth: { // video宽度，字符串类型，带单位
      type: String,
      default: '750rpx'
    },
    videoHeight: { // video高度，字符串类型，带单位
      type: String,
      default: '750rpx'
    },
    videoLoading: {
      type: String,
      default: true
    },
    src: {
      type: String,
      default: 'https://cv-vod.bhbapp.cn/cv-vod_517cca_1545386952360.mp4'
    },
    muted: {
      type: Boolean,
      default: true
    },
    autoplay: {
      type: Boolean,
      default: true
    },
    loop: {
      type: Boolean,
      default: true
    },
    objectFit: {
      type: String,
      default: 'cover'
    },
    showCenterPlayBtn: {
      type: String,
      default: true
    },
    controls: {
      type: Boolean,
      default: true
    }
  }
  data = {}
  onLoad () {
    this.drawLoadingOnCanvas()
  }

  drawLoadingOnCanvas () {
    const ctx = wepy.createCanvasContext('video-up-canvas')
    let width = this.videoWidth.search(/rpx/i) === -1 ? this.videoWidth.replace('px', '') * 1 : wepy.$minxins.toPx(this.videoWidth.replace('rpx', '') * 1)
    let height = this.videoHeight.search(/rpx/i) === -1 ? this.videoHeight.replace('px', '') * 1 : wepy.$minxins.toPx(this.videoHeight.replace('rpx', '') * 1)
    clearInterval(this.videoLoadingTimer)
    let rote = 0
    this.videoLoadingTimer = setInterval(() => {
      ctx.setFillStyle('#0E1730')
      ctx.fillRect(0, 0, width, height)
      ctx.save()
      ctx.beginPath()
      ctx.setLineWidth(3)
      ctx.setStrokeStyle('#404144')
      ctx.setLineCap('round')
      ctx.beginPath()
      ctx.arc(width / 2, height / 2, 15, 0, 2 * Math.PI)
      ctx.stroke()
      ctx.setLineWidth(3)
      ctx.setStrokeStyle('#999999')
      ctx.setLineCap('round')
      ctx.beginPath()
      ctx.arc(width / 2, height / 2, 15, rote * Math.PI, (1.6 + rote) * Math.PI)
      ctx.stroke()
      ctx.draw(false, () => {
        if (rote > 40) {
          clearInterval(this.videoLoadingTimer)
          this.stopLoadingOnCanvas()
        }
        rote = rote + 0.12
      })
    }, 50)
  }
  stopLoadingOnCanvas () {
    const ctx = wx.createCanvasContext('video-up-canvas')
    ctx.draw()
  }

  methods = {
    bindplay (e) {
      clearInterval(this.videoLoadingTimer)
      this.stopLoadingOnCanvas()
      this.$emit('bindplay', e)
    },
    bindpause (e) {
      this.$emit('bindpause', e)
    },
    bindwaiting (e) {
      this.$emit('bindwaiting', e)
    },
    binderror (e) {
      this.$emit('binderror', e)
    }
  }
  watch = {
    videoLoading (val) {
      if (val) {
        this.drawLoadingOnCanvas()
      }
    },
    src (val) {
      console.log('----val-->', val)
    }
  }
}
</script>

<style lang='scss'>
.video-component {
  position: relative;
  .el-video {
    width: 100%;
    height: 100%;
  }
  .video-up-canvas {
    position: absolute;
    top: 0rpx;
    left: 0rpx;
  }
}
</style>

```