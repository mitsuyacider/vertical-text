<template>
  <div id="app">
    <!-- <div class="lane-container" ref="container" :style="{fontSize: fontSize + 'px'}"/> -->
    <lane-container :column=column :fontSize=fontSize />
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
      fontSize: 20,
      column: 12
    }
  },
  mounted () {

  }
}
</script>

<style>
body {
  background: black;
  margin: 0;
  overflow: hidden;
}
/* .lane-container {
  text-align: center;
  display: inline-block;
} */
</style>
