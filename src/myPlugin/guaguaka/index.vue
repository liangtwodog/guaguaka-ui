<template>
  <div>
    <div class="guaguaka" :style="{width: guaguaka_width, height: guaguaka_height}">
      <img :src="curtainImg" class="loadImg prizeCurtainImg" alt="prizeCurtainImg"/>
      <img :src="prizeImg" class="loadImg prizeImg" alt="prize" v-show="scratchPrizeShow"/>
      <div class="touchSpot" :style="{width: touchSpotImgWidth, height: touchSpotImgHeight, backgroundImage: 'url(' + touchSpotImg + ')'}" v-show="touchSpotShow && ismousedown && isTouch"></div>
      <div class="touchSpotSizeDiv" :style="{width: touchSpotSize, height: touchSpotSize}" v-show="false"></div>
      <canvas class="canvas"></canvas>
    </div>
  </div>
</template>

<script>
import prizeCurtain from './img/prizeCurtain.png'
import $ from 'jquery'
export default {
  name:'guaguaka-ui',
  components:{},
  data(){
    return{
      prizeCurtain: '',
      ctx: '',
      ismousedown: false,
      scratchPrizeShow: false, // 刮刮卡奖品层是否显示
      dotPosition: null, // 起始点的位置
      isSuccess: true, // 是否开奖了
      touchSpotPositionY: '', // 触点Y轴的位置
      touchSpotPositionX: '', // 触点X轴的位置
      touchSpotSizeNb: '', // 涂抹的大小值
      itemDivId: {
        guaguaka: '',
        prizeCurtainImg: '',
        prizeImg: '',
        touchSpot: '',
        canvas: ''
      },
      id: null, // 刮刮卡id
      touchSpotShow: false, // 刮片是否显示
      touchSpotImg: 'https://i.loli.net/2019/01/17/5c3fef16bbd5a.png', // 刮片背景图
      isTouch: false, // 是否可以刮动,
      curtainImg: prizeCurtain, // 刮层图片
      prizeImg: 'https://i.loli.net/2019/01/17/5c3fef16bc433.png', // 奖品图片
      visibleArea: 50, // 刮开率,单位%
      touchSpotSize: '20px', // 涂抹的大小
      guaguaka_width: '246', // 刮刮卡的宽度
      guaguaka_height: '132', // 刮刮卡的高度
      touchSpotImgWidth: '20px', // 刮刮片的宽度,单位px
      touchSpotImgHeight: '20px', // 刮刮片的宽度,单位px
      touchSpotPosition: '0.5 0.5' // 刮片触点位置
    }
  },
  props: {
    guaguakaConfig: {
      type: Object,
      required: true,
      default: function () {
        return {}
      }
    }
  },
  mounted() {
    this.setGuaguaka(this.guaguakaConfig)
  },
  methods: {
    setGuaguaka(guaguakaInfo) {
      const that = this
      this.scratchPrizeShow = false
      if (guaguakaInfo) {
        if (!guaguakaInfo.id && this.id === null) {
          const error = new Error('请设置刮刮卡所在div的id')
          console.log(error)
          return '请设置刮刮卡所在div的id'
        } else {
          this.touchSpotImg = guaguakaInfo.touchSpotImg ? guaguakaInfo.touchSpotImg : this.touchSpotImg
          this.curtainImg = guaguakaInfo.curtainImg ? guaguakaInfo.curtainImg : this.curtainImg
          this.prizeImg = guaguakaInfo.prizeImg ? guaguakaInfo.prizeImg : this.prizeImg
          this.touchSpotShow = guaguakaInfo.touchSpotShow ? guaguakaInfo.touchSpotShow : this.touchSpotShow
          this.visibleArea = guaguakaInfo.visibleArea ? guaguakaInfo.visibleArea : this.visibleArea
          this.touchSpotSize = guaguakaInfo.touchSpotSize ? guaguakaInfo.touchSpotSize : this.touchSpotSize
          this.guaguaka_width = guaguakaInfo.width ? guaguakaInfo.width : this.guaguaka_width
          this.guaguaka_height = guaguakaInfo.height ? guaguakaInfo.height : this.guaguaka_height
          this.touchSpotImgWidth = guaguakaInfo.touchSpotImgWidth ? guaguakaInfo.touchSpotImgWidth : this.touchSpotImgWidth
          this.touchSpotImgHeight = guaguakaInfo.touchSpotImgHeight ? guaguakaInfo.touchSpotImgHeight : this.touchSpotImgHeight
          this.touchSpotPosition = guaguakaInfo.touchSpotPosition ? guaguakaInfo.touchSpotPosition : this.touchSpotPosition
          this.isTouch = guaguakaInfo.isTouch ? guaguakaInfo.isTouch : this.isTouch
          this.id = guaguakaInfo.id ? guaguakaInfo.id : this.id
        }
      }
      this.touchSpotPositionX = this.touchSpotPosition.split(' ')[0]
      this.touchSpotPositionY = this.touchSpotPosition.split(' ')[1]
      // 根据传入的刮刮卡div的id设置刮刮卡的组件各div的id
      $('#' + this.id).find('.guaguaka').attr('id', this.id + '_guaguaka')
      $('#' + this.id).find('.prizeCurtainImg').attr('id', this.id + '_prizeCurtainImg')
      $('#' + this.id).find('.prizeImg').attr('id', this.id + '_prizeImg')
      $('#' + this.id).find('.canvas').attr('id', this.id + '_canvas')
      if (this.touchSpotShow) {
        $('#' + this.id).find('.touchSpot').attr('id', this.id + '_touchSpot')
      }
      this.itemDivId = {
        guaguakaId: this.id + '_guaguaka',
        prizeCurtainImgId: this.id + '_prizeCurtainImg',
        prizeImgId: this.id + '_prizeImg',
        touchSpotId: this.id + '_touchSpot',
        canvasId: this.id + '_canvas'
      }
      $('#' + this.itemDivId.prizeCurtainImgId).attr('src', this.curtainImg)
      $('#' + this.itemDivId.prizeImgId).attr('src', this.prizeImg)
      const Interval = setInterval(function () {
        let imgLoad = 0
        $('#' + that.itemDivId.guaguakaId).find('.loadImg').each(function () {
          if (this.height !== 0) {
            imgLoad++
          }
        })
        if (imgLoad === 2) {
          // 当覆盖层图片和奖品图片加载完成就初始化刮刮卡
          that.init()
          window.clearInterval(Interval)
        }
      }, 300)
    },
    init() {
      this.prizeCurtain = document.getElementById(this.itemDivId.canvasId)
      this.prizeCurtain.width = this.prizeCurtain.clientWidth
      this.prizeCurtain.height = this.prizeCurtain.clientHeight
      this.ctx = this.prizeCurtain.getContext('2d')
      // PC端的处理
      this.prizeCurtain.addEventListener('mousemove', this.canvasEventMove, true)
      this.prizeCurtain.addEventListener('mousedown', this.canvasEventDown, true)
      // 移动端的处理
      this.prizeCurtain.addEventListener('touchstart', this.canvasEventDown, false)
      this.prizeCurtain.addEventListener('touchmove', this.canvasEventMove, false)

      this.prizeCurtain.addEventListener('touchend', this.canvasEventUp, false)
      document.addEventListener("mouseup", this.canvasEventUp, true)
      this.ctx.globalCompositeOperation = 'source-over'
      this.setPrizeCurtain()
    },
    setPrizeCurtain () {
      this.ctx.globalCompositeOperation = 'source-over'
      const img = document.getElementById(this.itemDivId.prizeCurtainImgId)
      this.ctx.drawImage(img, 0, 0, this.prizeCurtain.width, this.prizeCurtain.height)
      // 有些老的手机自带浏览器不支持destination-out,下面的代码中有修复的方法
      this.ctx.globalCompositeOperation = 'destination-out'
      this.isSuccess = false
      this.scratchPrizeShow = true
      this.$emit('hideLoading', this.id)
    },
    canvasEventUp(e) {
      e.preventDefault()
      // 判断刮刮卡是否已经刮开率是否超过了设置的刮开率，如果是的话就不在不返回成功的
      if (!this.isSuccess) {
        var a = this.ctx.getImageData(0, 0, this.prizeCurtain.width, this.prizeCurtain.height)
        var j = 0
        for (var i = 3; i < a.data.length; i += 4) {
          if (a.data[i] === 0)j++
        }
        // 当被刮开的区域等于刮开率时，则可以开始处理结果
        if (j * 100 / this.visibleArea >= a.data.length / 4) {
          // alert('刮卡成功')
          this.isSuccess = true
          this.$emit('success', this.id)
        }
      }
      this.ismousedown = false
      this.dotPosition = null
    },
    canvasEventMove(e) {
      e.preventDefault()
      if (this.ismousedown && this.isTouch && this.scratchPrizeShow) {
        if (e.changedTouches) {
          e = e.changedTouches[e.changedTouches.length - 1]
        }
        // 获取当前滚动条的位置
        const scrollTop = $(document).scrollTop()
        const scrollLeft = $(document).scrollLeft()
        // 获取刮刮卡相对整个屏幕右上角的位置
        const oX = $('#' + this.itemDivId.guaguakaId).offset().left
        const oY = $('#' + this.itemDivId.guaguakaId).offset().top
        // 计算手指在刮刮卡上的位置
        const x = (e.clientX + scrollLeft || e.pageX) - oX || 0
        const y = (e.clientY + scrollTop || e.pageY) - oY || 0
        // 设置刮片的位置
        $('#' + this.itemDivId.touchSpotId).css({
          left: x - $('#' + this.itemDivId.touchSpotId).width() * this.touchSpotPositionX,
          top: y - $('#' + this.itemDivId.touchSpotId).height() * this.touchSpotPositionY
        })
        if (this.dotPosition === null) {
          this.dotPosition = {
            x: x,
            y: y
          }
        } else {
          // 根据刮片起始点和当前的点来画出来是透明的线
          this.ctx.beginPath()
          this.ctx.lineWidth = this.touchSpotSizeNb
          this.ctx.lineCap = 'round'
          this.ctx.lineJoin = 'round'
          this.ctx.moveTo(this.dotPosition.x, this.dotPosition.y)
          this.ctx.lineTo(x, y)
          // 下面2行代码是为了修复部分手机浏览器不支持destination-out
          // 我也不是很清楚这样做的原理是什么
          this.prizeCurtain.style.display = 'none'
          this.prizeCurtain.style.display = 'inherit'
          this.ctx.stroke()
          // 设置刮片起始点
          this.dotPosition = {
            x: x,
            y: y
          }
          // this.ctx.arc(x, y, this.touchSpotSize * 0.2, 0, Math.PI * 2)
          // this.ctx.fill()
        }
      }
    },
    canvasEventDown(e) {
      e.preventDefault()
      if (this.isTouch) {
        // 获取当前滚动条的位置
        const scrollTop = $(document).scrollTop()
        const scrollLeft = $(document).scrollLeft()
        // 获取刮刮卡相对整个屏幕右上角的位置
        const oX = $('#' + this.itemDivId.guaguakaId).offset().left
        const oY = $('#' + this.itemDivId.guaguakaId).offset().top
        // 计算手指在刮刮卡上的位置
        const x = (e.clientX + scrollLeft || e.pageX) - oX || 0
        const y = (e.clientY + scrollTop || e.pageY) - oY || 0
        // 设置刮片的位置
        $('#' + this.itemDivId.touchSpotId).css({
          left: x - $('#' + this.itemDivId.touchSpotId).width() * this.touchSpotPositionX,
          top: y - $('#' + this.itemDivId.touchSpotId).height() * this.touchSpotPositionY
        })
        this.touchSpotSizeNb = $('#' + this.id).find('.touchSpotSizeDiv').width()
        this.dotPosition = {
          x: x,
          y: y
        }
        this.ismousedown = true
      }
    },
    setInfo(guaguakaInfo) { // 设置奖品图片
      const that = this
      that.touchSpotImg = guaguakaInfo.touchSpotImg ? guaguakaInfo.touchSpotImg : that.touchSpotImg
      that.touchSpotImgWidth = guaguakaInfo.touchSpotImgWidth ? guaguakaInfo.touchSpotImgWidth : that.touchSpotImgWidth
      that.touchSpotImgHeight = guaguakaInfo.touchSpotImgHeight ? guaguakaInfo.touchSpotImgHeight : that.touchSpotImgHeight
      that.touchSpotPosition = guaguakaInfo.touchSpotPosition ? guaguakaInfo.touchSpotPosition : that.touchSpotPosition
      that.touchSpotSize = guaguakaInfo.touchSpotSize ? guaguakaInfo.touchSpotSize : that.touchSpotSize
      that.touchSpotPositionX = that.touchSpotPosition.split(' ')[0]
      that.touchSpotPositionY = that.touchSpotPosition.split(' ')[1]
      if (guaguakaInfo.isTouch) {
        that.isTouch = true
      } else {
        that.isTouch = false
      }
      if (guaguakaInfo.prizeImg) {
        // 隐藏奖品图片层，防止当奖品图片加载完成后用户会看到奖品
        that.scratchPrizeShow = false
        // 设置奖品图片
        this.prizeImg = guaguakaInfo.prizeImg
        const Interval = setInterval(function () {
          let imgLoad = 0
          // 判断奖品图片是否加载完成
          if ($('#' + that.itemDivId.prizeImgId).height !== 0) {
            imgLoad++
          }
          if (imgLoad === 1) {
            // 重置刮刮卡覆盖层
            that.setPrizeCurtain()
            window.clearInterval(Interval)
          }
        }, 300)
      }
    }
  }
}
</script>

<style scoped>
  .guaguaka{
    position: relative;
    overflow: hidden;
  }
  .guaguaka .prizeCurtainImg{
    display: none;
  }
  .guaguaka .prizeImg{
    width: 100%;
    height: 100%;
  }
  .guaguaka .canvas{
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 100;
  }
  .guaguaka .touchSpot{
    position: absolute;
    left: 0;
    top: 0;
    z-index: 110;
    background-repeat: no-repeat;
    background-size: 100% 100%;
  }
</style>
