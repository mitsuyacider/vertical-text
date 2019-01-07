<template>
  <div class="exp" :class="this.laneData.className">{{ this.laneData.sentence }}</div>
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
  computed: {
    screenWidth () {
      return this.isDebug ? window.innerWidth : window.screen.width / 2
    },
    screenHeight () {
      return this.isDebug ? window.innerHeight : window.screen.height
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
      const elements = document.getElementsByClassName(this.laneData.className)
      const element = elements[0]
      element.style.left = this.laneData.x + 'px'
      console.log(window.screen.height)
      element.style.top = this.screenHeight + 'px'
      element.style.width = this.laneData.fontSize + 'px'
      element.fontSize = this.laneData.fontSize + 'px'
      element.style.height = this.laneData.sentence.length * 12 * 2 + 'px'
    },
    startAnimation () {
      const random = Math.random() * 1000
      const self = this
      const endY = -this.screenHeight - this.laneData.sentence.length * 12 * 2
      const absEndY = Math.abs(endY)
      anime({
        targets: '.' + this.laneData.className,
        delay: random,
        translateY: { value: endY },
        duration: 15000,
        easing: 'linear',
        update: function (anim) {
          const elements = document.getElementsByClassName(self.laneData.className)
          const transformStyle = elements[0].style.transform

          // NOTE: transformStyleには「translateY(xxxpx)」という値が入っているため、
          //       数値部分のみ抜き出す
          const val = transformStyle.replace(/[^\d.]/g, '')
          self.update(self.laneData, val)

          // NOTE: 画面から全てのテキストがフレームアウトしたら、
          //       アニメーション終了とみなす
          if (val >= absEndY) {
            self.complete(self.laneData, elements[0])
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
