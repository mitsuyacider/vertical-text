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
		laneData: {
			type: Object
		}
	},
	mounted () {
		this.setPosition(this.laneData.x)
		this.startAnimation()
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
		setPosition (x) {
			const elements = document.getElementsByClassName(this.laneData.className);
			const element = elements[0]
				element.style.left = x + 'px'
			element.style.top = this.screenHeight + 'px'
			element.style.height = this.laneData.sentence.length * 12 * 2 + 'px'
		},
		startAnimation () {
			const random = Math.random() * 1000
			const self = this
			const endY = -this.screenHeight - this.laneData.sentence.length * 12 * 2
			const absEndY = Math.abs(endY)
			const basicTimeline = anime({
				targets: '.' + this.laneData.className,
					delay: random,
					translateY: { value: endY },
					duration: 15000,
					easing: 'linear',
					update: function(anim){
						const elements = document.getElementsByClassName(self.laneData.className);
						const transformStyle = elements[0].style.transform
						const val = transformStyle.replace(/[^\d.]/g, '')
						if (val >= absEndY) {
							self.$emit('callback', self.laneData)
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
	color: white;
  /* NOTE: Vertical line */
  writing-mode: vertical-rl;
  -webkit-writing-mode: vertical-rl;
  -moz-writing-mode: vertical-rl;
  -ms-writing-mode: tb-rl;
  -ms-writing-mode: vertical-rl;
}
</style>