<template>
  <div class="lane-container" ref="container" :style="{fontSize: fontSize + 'px'}"/>
</template>

<script>
import anime from 'animejs'
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
			const x = (this.fontSize + this.lineSpace) * i
			const laneData = {
				x: x,
				className: 'lane' + i,
				laneId: i,
				primaryId: i,
				sentence: this.sentenceList[i] + i
			}
			
      const ComponentClass = Vue.extend(VerticalLane)
      const instance = new ComponentClass({
          propsData: { 
						delegate: this.callbackOnVerticalLane,
						laneData: laneData
					}
      })
      instance.$mount() // pass nothing
      this.$refs.container.appendChild(instance.$el)
      instance.setPosition(x)
			instance.startAnimation()
    }  
	},
	methods: {
		callbackOnVerticalLane (laneData, node) {
			const x = (this.fontSize + this.lineSpace) * laneData.laneId
			const newId = laneData.primaryId + this.column
			const data = {
				sentence: this.sentenceList[laneData.laneId],
				className: 'lane' + newId,
				laneId: laneData.laneId,
				x: x,
				primaryId: newId
			}

      const ComponentClass = Vue.extend(VerticalLane)
      const instance = new ComponentClass({
          propsData: { 
						delegate: this.callbackOnVerticalLane,
						laneData: data
					}
      })
      instance.$mount() // pass nothing
      this.$refs.container.appendChild(instance.$el)
      instance.setPosition(x)
			instance.startAnimation()		
			
			this.$refs.container.removeChild(node)

			console.log(this.$refs.container)
		}
	}
}
</script>

<style scoped>
.lane-container {
	overflow: hidden;
	color: lightgrey;
}
</style>
