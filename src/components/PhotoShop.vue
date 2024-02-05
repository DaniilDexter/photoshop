<template>
  <div class="photoshop">
    <HeaderPanel @download="callback" class="header"/>
    <div class="wrapper">
      <SidebarPanel class="sidebar"/>
      <canvas class="drawing" ref="drawing"/>
      <img src="@/assets/logo.png" class="source" alt="" ref="source">
    </div>
  </div>
</template>

<script>
import HeaderPanel from './HeaderPanel.vue'
import SidebarPanel from './SidebarPanel.vue'
export default {
  name: 'PhotoShop',
  props: {
    msg: String
  },
  components: {
    SidebarPanel,
    HeaderPanel
  },
  data() {
    return {
      canvas: null,
      source: null
    }
  },
  mounted() {
    this.canvas = this.$refs["drawing"].getContext('2d')
    this.source = this.$refs["source"]
  },
  methods: {
    callback(){
      this.canvas.drawImage(this.source, 0, 0)
      console.log(this.canvas.getImageData(0,2,1,1))
    }
  }
}
</script>

<style lang="scss" scoped>
.photoshop{
  height: 100%;
}
.wrapper{
  display: flex;
  flex-direction: row;
  height: calc(100% - 80px);
}
.source{
  display: none;
}
#myCanvas {
  border: 1px solid grey;
}
</style>
