<template>
  <div id="app">
    <!-- <div class="lane-container" ref="container" :style="{fontSize: fontSize + 'px'}"/> -->
    <lane-container />
  </div>
</template>

<script>
import LaneContainer from '@/components/LaneContainer'

export default {
  name: 'App',
  components: {
    LaneContainer
  },
  data () {
    return {
      fontSize: 10
    }
  },
  mounted () {

  }
}
</script>

<style>
/* .lane-container {
  text-align: center;
  display: inline-block;
} */
</style>
