<template>
  <div class="exp" :class="this.laneData.className">
    <span>{{ this.laneData.sentence }}</span>
  </div>
</template>

<script>
import anime from 'animejs'

export default {
  name: 'VerticalLane',
  props: {
    // NOTE: This property may be called from storybook
    isDebug: {
      type: Boolean,
      default: false
    },
    update: {
      type: Function
    },
    complete: {
      type: Function
    },
    laneData: {
      type: Object
    }
  },
  data () {
    return {
      shouldNotify: true
    }
  },
  computed: {
    screenWidth () {
      return this.isDebug ? window.innerWidth : window.screen.width / 2
    },
    screenHeight () {
      return this.isDebug ? window.innerHeight : window.screen.height
    },
    rootElement () {
      const elements = document.getElementsByClassName(this.laneData.className)
      return elements[0]
    }
  },
  methods: {
    initialize () {
      this.setPosition()
      this.startAnimation()
    },
    getTestData () {
      return 'test'
    },
    setPosition () {
      this.rootElement.style.left = this.laneData.x + 'px'
      this.rootElement.style.top = this.screenHeight + 'px'
      this.rootElement.style.width = this.laneData.fontSize + 'px'
      this.rootElement.fontSize = this.laneData.fontSize + 'px'
      this.rootElement.style.height = this.laneData.sentence.length * this.laneData.fontSize + 'px'
    },
    startAnimation () {
      const self = this

      // NOTE: 1秒間に1ピクセル
      const speed = 60
      const contentsHeight = this.rootElement.children[0].getBoundingClientRect().height
      const endY = Math.floor(-1 * (this.screenHeight + contentsHeight) - this.laneData.fontSize)
      const absEndY = Math.abs(endY)
      const duration = absEndY / speed * 1000
      anime({
        targets: '.' + this.laneData.className,
        delay: 0,
        translateY: { value: endY },
        duration: duration,
        easing: 'linear',
        update: function (anim) {
          const transformStyle = self.rootElement.style.transform

          // NOTE: transformStyleには「translateY(xxxpx)」という値が入っているため、
          //       数値部分のみ抜き出す
          const val = transformStyle.replace(/[^\d.]/g, '')

          // NOTE: テキストがすべて出現し切ったら、次のテキストを表示させる
          if (val > contentsHeight && self.shouldNotify) {
            self.update(self.laneData, val, self.rootElement)
            self.shouldNotify = false
          }

          // NOTE: 画面から全てのテキストがフレームアウトしたら、
          //       アニメーション終了とみなす
          if (val >= absEndY) {
            self.complete(self.laneData, self.rootElement)
          }
        }
      })
    }
  }
}
</script>
<style scoped>
.exp {
  position: absolute;

  /* NOTE: Vertical line */
  writing-mode: vertical-rl;
  -webkit-writing-mode: vertical-rl;
  -moz-writing-mode: vertical-rl;
  -ms-writing-mode: tb-rl;
  -ms-writing-mode: vertical-rl;
}
</style>
