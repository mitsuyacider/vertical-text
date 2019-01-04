<template>
  <div class="lane-container" ref="container" :style="{fontSize: fontSize + 'px'}"/>
</template>

<script>
import anime from 'animejs'
import VerticalLane from '@/components/VerticalLane'
import Vue from 'vue'

export default {
  data () {
    return {
      fontSize: 10
    }
  },
  mounted () {

    for (let i = 0; i < 10; i++) {
      var ComponentClass = Vue.extend(VerticalLane)
      var instance = new ComponentClass({
          propsData: { 
            sentence: 'この作品の評価は高く、多くの鑑賞者から絶賛されています。',
            className: 'lane' + i }
      })
      instance.$mount() // pass nothing
      this.$refs.container.appendChild(instance.$el)
      instance.setPositionX(i * 50)
      instance.startAnimation()
    }  
  }	
}
</script>

<style scoped>
.lane-container {
  text-align: center;
  display: inline-block;
}
</style>
