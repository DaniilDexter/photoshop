<template>
  <div class="photoshop">
    <HeaderPanel @download="callback" @show="changeModal" class="header" ref="header" />
    <div class="wrapper">
      <SidebarPanel
        class="sidebar"
        :x="this.x"
        :y="this.y"
        :res="this.resl"
        ref="sidebar"
      />
      <div class="drawing">
        <canvas
          width="1000"
          height="600"
          ref="drawing"
          @mousemove="showCoordinates"
          @click="save"
        />
      </div>
    </div>
    <DownloadModal v-show="showModal" @show="changeModal"/>
  </div>
</template>

<script>
import HeaderPanel from "./HeaderPanel.vue";
import SidebarPanel from "./SidebarPanel.vue";
import DownloadModal from "./DownloadModal.vue"
export default {
  name: "PhotoShop",
  components: {
    SidebarPanel,
    HeaderPanel,
    DownloadModal,
  },
  data() {
    return {
      canvas: null,
      ctx: null,
      resl: [],
      x: 0,
      y: 0,
      showModal: false
    };
  },
  mounted() {
    this.canvas = this.$refs["drawing"];
    this.ctx = this.canvas.getContext("2d", { willReadFrequently: true });
  },
  methods: {
    callback() {
      this.ctx.drawImage(this.$refs.header.new, 0, 0, 1000, 600);
    },
    showCoordinates(e) {
      this.x = e.offsetX;
      this.y = e.offsetY;
    },
    save() {
      console.log("click");
      this.resl = this.ctx.getImageData(this.x, this.y, 1, 1).data;
    },
    start() {
      console.log("start");
    },
    changeModal() {
      this.showModal = !this.showModal
    }
  },
};
</script>

<style lang="scss" scoped>
.photoshop {
  height: 100%;
}
.wrapper {
  display: flex;
  flex-direction: row;
  height: calc(100% - 80px);
}
.source {
  display: none;
}
#myCanvas {
  border: 1px solid grey;
}

.drawing {
  height: 600px;
  width: 1000px;
}
</style>
