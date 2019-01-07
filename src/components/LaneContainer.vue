<template>
  <!-- <div class="lane-container" ref="container" :style="{fontSize: fontSize + 'px'}"/> -->
	<div class="main-viertical-container">
		<div v-for="laneData in laneDataList" :key="laneData.laneId">
			<vertical-lane :laneData=laneData v-on:callback=callbackOnVerticalLane />
    	<!-- {{ item.message }} -->
  	</div>
	</div>
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
    },
	},
	data () {
		return {
			laneDataList: [],
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
	components: {
		VerticalLane
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
		// this.fontSize = Math.floor((this.screenWidth - this.column * this.lineSpace) / this.column)
    for (let i = 0; i < this.column; i++) {
			const x = (this.fontSize + this.lineSpace) * i
			const data = {
				sentence: this.sentenceList[i],
				className: 'lane' + i,
				laneId: i,
				x: x,
				primaryId: i
			}
			this.laneDataList.push(data)
			// const x = (this.fontSize + this.lineSpace) * i
      // const ComponentClass = Vue.extend(VerticalLane)
      // const instance = new ComponentClass({
      //     propsData: { 
      //       sentence: sentenceList[i] + i,
			// 			className: 'lane' + i,
			// 		}
			// })
      // instance.$mount() // pass nothing
      // this.$refs.container.appendChild(instance.$el)
      // instance.setPosition(x)
			// instance.startAnimation()
			// instance["v-on"] = {
			// 	callback (name) {
			// 		console.log("*** listeners")
			// 	}				
			// }
    }  
	},
	methods: {
		callbackOnVerticalLane(laneData) {
			if (this.laneDataList.length > 0) {
				const index = this.laneDataList.findIndex(({ primaryId }) => primaryId === laneData.primaryId);
				console.log(laneData.className + " / index : " + index)
				this.laneDataList.splice(index, 1);
				const x = (this.fontSize + this.lineSpace) * laneData.laneId
				const newId = laneData.primaryId + this.column
				const data = {
					sentence: this.sentenceList[laneData.laneId],
					className: 'lane' + newId,
					laneId: laneData.laneId,
					x: x,
					primaryId: newId
				}
				this.laneDataList.push(data)
				
			}
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
