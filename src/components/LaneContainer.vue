<template>
  <div class="lane-container" ref="container" />
</template>

<script>
import VerticalLane from '@/components/VerticalLane'
import Vue from 'vue'

export default {
  props: {
    column: {
      type: Number,
      default: 12
    },
    lineSpace: {
      type: Number,
      default: 8
    },
    maxSpeed: {
      type: Number,
      default: 100
    },
    minSpeed: {
      type: Number,
      default: 50
    }
  },
  data () {
    return {
      fontSize: 14,
      sentenceList: [
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。',
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
      injectedCnt: 0,
      laneSpeedStore: []
    }
  },
  mounted () {
    this.fontSize = this.screenWidth / this.column - this.lineSpace
    for (let i = 0; i < this.column; i++) {
      // NOTE: Math.random() * (最大値 - 最小値) + 最小値
      const speed = Math.floor(Math.random() * (this.maxSpeed - this.minSpeed) + this.minSpeed)
      this.laneSpeedStore.push(speed)
      const isFirstLane = true
      this.createLaneComponent(i, isFirstLane)
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
      const isFirstLane = false
      this.createLaneComponent(laneData.laneId, isFirstLane)
    },
    onCompleteLaneAnimation (laneData, node) {
      this.$refs.container.removeChild(node)
      this.updateInjectedCnt()
    },
    createLaneComponent (laneId, isFirstLane) {
      const data = this.createLaneData(laneId)
      const ComponentClass = Vue.extend(VerticalLane)
      const instance = new ComponentClass({
        propsData: {
          update: this.onUpdateLaneAnimation,
          complete: this.onCompleteLaneAnimation,
          laneData: data,
          isFirstLane: isFirstLane
        }
      })

      instance.$mount()
      this.$refs.container.appendChild(instance.$el)

      // NOTE: mountした段階だとまだpropsDataがundefinedなため、
      //       appendChildした後で別途で初期化処理を行う
      instance.initialize()
    },
    createLaneData (laneId) {
      const offsetX = (this.screenWidth - ((this.fontSize + this.lineSpace) * this.column)) / 2 + this.lineSpace / 2
      const x = (this.fontSize + this.lineSpace) * laneId + offsetX
      const primaryKey = new Date().getTime().toString(16) + Math.floor(2000 * Math.random()).toString(16)
      const data = {
        sentence: this.sentenceList[this.injectedCnt],
        className: 'lane' + primaryKey,
        laneId: laneId,
        x: x,
        speed: this.laneSpeedStore[laneId],
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
