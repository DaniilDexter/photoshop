<template>
  <div class="photoshop">
    <HeaderPanel
      @show="changeModal"
      @clear="clear(), closeDisplay()"
      class="header"
      ref="header"
    />
    <div class="wrapper">
      <SidebarPanel
        class="sidebar"
        :x="this.x"
        :y="this.y"
        :height="this.height"
        :width="this.width"
        :res="this.resl"
        :showData="this.showData"
        ref="sidebar"
      />
      <div class="drawing">
        <canvas
          width="1163"
          height="564"
          ref="drawing"
          @mousemove="showCoordinates"
          @click="save()"
        />
        <div v-if="resl != null" class="color">
          <div class="color__img" :style="string"></div>
          <div class="color__inf">
            <p class="color__font">{{ color }}</p>
            <p class="color__font">Hex: {{ HEX }}</p>
          </div>
        </div>
      </div>
    </div>
    <DownloadModal
      v-show="showModal"
      @show="changeModal"
      @download="clear(), draw(), showDisplay()"
      ref="modal"
    />
  </div>
</template>

<script>
import HeaderPanel from "./HeaderPanel.vue";
import SidebarPanel from "./SidebarPanel.vue";
import DownloadModal from "./DownloadModal.vue";
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
      resl: null,
      x: 0,
      y: 0,
      showModal: false,
      showData: false,
      height: 0,
      width: 0,
    };
  },
  mounted() {
    this.canvas = this.$refs["drawing"];
    this.ctx = this.canvas.getContext("2d", { willReadFrequently: true });
  },
  methods: {
    draw() {
      this.height = this.$refs.modal.result.height;
      this.width = this.$refs.modal.result.width;
      let scale_factor = Math.min(
        this.canvas.width / this.width,
        this.canvas.height / this.height
      );
      let newWidth = this.width * scale_factor;
      let newHeight = this.height * scale_factor;
      let x = this.canvas.width / 2 - newWidth / 2;
      let y = this.canvas.height / 2 - newHeight / 2;
      this.ctx.drawImage(this.$refs.modal.result, x, y, newWidth, newHeight);
    },
    showCoordinates(e) {
      this.x = e.offsetX;
      this.y = e.offsetY;
    },
    save() {
      if(this.$refs.modal.result != null){
        this.resl = this.ctx.getImageData(this.x, this.y, 1, 1).data;
      }
    },
    changeModal() {
      this.showModal = !this.showModal;
    },
    showDisplay() {
      this.showData = true;
    },
    closeDisplay() {
      this.showData = false;
    },
    clear() {
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
      this.resl = null
    },
  },
  computed: {
    string() {
      return (
        "background-color: rgb(" +
        this.resl[0] +
        "," +
        this.resl[1] +
        "," +
        this.resl[2] +
        ");"
      );
    },
    color() {
      return (
        "rgb(" + this.resl[0] + ", " + this.resl[1] + ", " + this.resl[2] + ")"
      );
    },
    HEX() {
      return (
        "#" +
        [this.resl[0], this.resl[1], this.resl[2]]
          .map((x) => {
            const hex = x.toString(16);
            return hex.length === 1 ? "0" + hex : hex;
          })
          .join("")
      );
    },
  },
};
</script>

<style lang="scss" scoped>
.photoshop {
  height: 100%;
}
.color {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  column-gap: 20px;
  margin-top: 10px;

  &__img{
  height: 40px;
  width: 40px;
  border-radius: 50%;
  border: 1px solid #e0e1dd;
  }

  &__font{
    color: #e0e1dd;
  }

  &__inf{
    display: flex;
    flex-direction: column;
    row-gap: 10px;
  }
}
.wrapper {
  display: flex;
  flex-direction: row;
  height: calc(100% - 80px);
}
.source {
  display: none;
}

.drawing {
  height: 565.5px;
  width: 1165px;
  background-image: url("@/assets/1674303626_catherineasquithgallery-com-p-fon-serii-kvadratiki-foto-22.jpg");
  border: 1px solid #e0e1dd;
}
</style>
