<template>
  <div class="content">
    <p class="font">{{ x }}, {{ y }}</p>
    <div class="color" :style="string" ref="picked"></div>
    <p class="font">{{ color }}</p>
  </div>
</template>
  
  <script>
export default {
  name: "SidebarPanel",
  props: {
    x: Number,
    y: Number,
    res: Uint8ClampedArray,
  },
  data() {
    return {
      picker: null,
    }
  },
  mounted() {
    this.picker = this.$refs["picked"]
  },
  methods: {
    // changecolor(){
    //   let string = "background-color: rgb(" + this.res[0] + "," + this.res[1] + "," + this.res[2] + ");"
    //   // this.picker.style.cssText = string
    // }
  },
  computed: {
    string(){
      return "background-color: rgb(" + this.res[0] + "," + this.res[1] + "," + this.res[2] + ");"
    },
    color(){
      return "rgb(" + this.res[0] + "," + this.res[1] + "," + this.res[2] + ")"
    }
  }
};
</script>
  
  <style lang="scss" scoped>
.content {
  width: 100px;
  height: 100%;
  border-right: 1px solid black;
  background-color: #1b263b;
}
.color{
  height: 30px;
  width: 30px;
  border-radius: 15px;
  border: 1px solid #e0e1dd;
}
.font{
  color: #e0e1dd;
}
</style>
  