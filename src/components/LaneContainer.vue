<template>
  <div class="lane-container" ref="container" :style="{fontSize: fontSize + 'px'}"/>
</template>

<script>
import VerticalLane from '@/components/VerticalLane'
import Vue from 'vue'

export default {
  props: {
    fontSize: {
      type: Number,
      default: 12
    },
    column: {
      type: Number,
      default: 12
    },
    lineSpace: {
      type: Number,
      default: 14
    }
  },
  data () {
    return {
      sentenceList: [
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。',
        'この作品の評価は高く、多くの鑑賞者から絶賛されています。この作品の評価は高く、多くの鑑賞者から絶賛されています。'
      ]
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
  mounted () {
    for (let i = 0; i < this.column; i++) {
      this.createLaneComponent(i)
    }
  },
  methods: {
    onUpdateLaneAnimation (laneData) {
      this.createLaneComponent(laneData.laneId)
    },
    onCompleteLaneAnimation (node) {
      this.$refs.container.removeChild(node)
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
      const primaryKey = new Date().getTime().toString(16) + Math.floor(1000 * Math.random()).toString(16)
      const data = {
        sentence: this.sentenceList[laneId],
        className: 'lane' + primaryKey,
        laneId: laneId,
        x: x
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
