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
      const instance = this.createLaneComponent(i)
      instance.$mount()
      this.$refs.container.appendChild(instance.$el)
      instance.setPosition()
      instance.startAnimation()
    }
  },
  methods: {
    callbackOnVerticalLane (laneData, node) {
      const instance = this.createLaneComponent(laneData.laneId)
      instance.$mount()
      this.$refs.container.appendChild(instance.$el)
      instance.setPosition()
      instance.startAnimation()
      this.$refs.container.removeChild(node)
      console.log(this.$refs.container)
    },
    createLaneComponent (laneId) {
      const data = this.createLaneData(laneId)
      const ComponentClass = Vue.extend(VerticalLane)
      const instance = new ComponentClass({
        propsData: {
          delegate: this.callbackOnVerticalLane,
          laneData: data
        }
      })
      return instance
    },
    createLaneData (laneId) {
      const x = (this.fontSize + this.lineSpace) * laneId
      const primaryKey = new Date().getTime().toString(16)
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
