<template>
  <div class="exp" :class="className">{{ this.sentence }}</div>
</template>

<script>
import anime from 'animejs'

export default {
  name: 'VerticalLane',
  props: {
    lineSpace: {
      type: Number,
      default: 5
    },
    // NOTE: This property may be called from storybook
    isDebug: {
      type: Boolean,
      default: false
		},
		sentence: {
			type: String,
			default: 'メディア芸術祭'
		},
		className: {
			type: String,
			default: 'lane0'
		}
	},
	mounted () {
		console.log("** mounted **");
	},
	methods: {
		setPositionX (x) {
			const elements = document.getElementsByClassName(this.className);
			elements[0].style.left = x + 'px'
		},
		startAnimation () {
			const random = Math.random() * 1000
			const self = this
			const basicTimeline = anime({
				targets: '.' + this.className,
					delay: random,
					translateY: { value: '-100vh' },
					duration: 5000,
					easing: 'linear',
					update: function(anim){
						const elements = document.getElementsByClassName('lane0');
						const transformStyle = elements[0].style.transform
						const val = transformStyle.replace(/[^\d.]/g, '')
						console.log(val)
    			}
			});
		}
	}
}
</script>
<style scoped>
.exp {
	position: absolute;
	bottom: -100vh;
	/* position: absolute; */
  /* NOTE: Vertical line */
  writing-mode: vertical-rl;
  -webkit-writing-mode: vertical-rl;
  -moz-writing-mode: vertical-rl;
  -ms-writing-mode: tb-rl;
  -ms-writing-mode: vertical-rl;
  
  display: inline-block;
  height: 100vh;
  text-align: left;
}
</style>