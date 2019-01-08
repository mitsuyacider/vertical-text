<template>
  <div class="lane-container" ref="container" />
</template>

<script>
import VerticalLane from '@/components/VerticalLane'
import Vue from 'vue'

export default {
  props: {
    // fontSize: {
    //   type: Number,
    //   default: 12
    // },
    column: {
      type: Number,
      default: 12
    },
    lineSpace: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      fontSize: 14,
      sentenceList: [
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。0000aaaa',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。---|||1111bbbb',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。2222CCCC',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。3333DDDD',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。4444EEEE',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。5555ffff',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。6666ggggg',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。7777hhhh',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。88888iiiii',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。9999ooooo',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。10101010asfdsaf',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。111fjaksfja'
      ],
      injectedCnt: 0
    }
  },
  mounted () {
    this.fontSize = Math.floor(this.screenWidth / this.column)
    for (let i = 0; i < this.column; i++) {
      this.createLaneComponent(i)
      this.updateInjectedCnt()
    }
  },
  computed: {
    screenWidth () {
      return this.isDebug ? window.innerWidth : window.screen.width
    },
    screenHeight () {
      return this.isDebug ? window.innerHeight : window.screen.height
    }
  },
  methods: {
    updateInjectedCnt () {
      this.injectedCnt++
      if (this.injectedCnt > this.sentenceList.length - 1) {
        this.injectedCnt = 0
      }
    },
    onUpdateLaneAnimation (laneData, val, node) {
      this.createLaneComponent(laneData.laneId)
    },
    onCompleteLaneAnimation (laneData, node) {
      this.$refs.container.removeChild(node)
      this.updateInjectedCnt()
    },
    createLaneComponent (laneId) {
      const data = this.createLaneData(laneId)
      const ComponentClass = Vue.extend(VerticalLane)
      const instance = new ComponentClass({
        propsData: {
          update: this.onUpdateLaneAnimation,
          complete: this.onCompleteLaneAnimation,
          laneData: data
        }
      })

      instance.$mount()
      this.$refs.container.appendChild(instance.$el)

      // NOTE: mountした段階だとまだpropsDataがundefinedなため、
      //       appendChildした後で別途で初期化処理を行う
      instance.initialize()
    },
    createLaneData (laneId) {
      const x = (this.fontSize + this.lineSpace) * laneId
      const primaryKey = new Date().getTime().toString(16) + Math.floor(2000 * Math.random()).toString(16)
      const data = {
        sentence: this.sentenceList[this.injectedCnt],
        className: 'lane' + primaryKey,
        laneId: laneId,
        x: x,
        speed: 64,
        fontSize: this.fontSize
      }

      return data
    }
  }
}
</script>

<style scoped>
.lane-container {
  overflow: hidden;
  color: white;
}
</style>
