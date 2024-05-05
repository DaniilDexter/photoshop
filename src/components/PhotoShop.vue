<template>
  <div class="photoshop">
    <HeaderPanel
      @show="changeUploadModal"
      @clear="clear(), closeDisplay()"
      @scale="changeScaleModal"
      @save="saveImage"
      class="header"
      ref="header"
    />
    <div class="wrapper">
      <SidebarPanel
        class="sidebar"
        @scale="scaleImage()"
        :x="this.x"
        :y="this.y"
        :height="this.height"
        :width="this.width"
        :resl="this.resl"
        :showData="this.showData"
        ref="sidebar"
      />
      <div class="drawing">
        <canvas
          width="1250"
          height="640"
          ref="drawing"
          @mousemove="showCoordinates"
          @click="save()"
        />
      </div>
    </div>
    <div style="display: none;">
      <canvas ref="canvasHelper"/>
    </div>
    <UploadModal
      v-show="showUploadModal"
      @show="changeUploadModal"
      @download="clear(), draw(), showDisplay()"
      ref="modal"
    />
    <ScaleModal
      :nowH="this.nowH"
      :nowW="this.nowW"
      v-show="showScaleModal"
      @show="changeScaleModal"
      ref="scaleModal"
      @resize="resize"
    />
  </div>
</template>

<script>
import HeaderPanel from "./HeaderPanel.vue";
import SidebarPanel from "./SidebarPanel.vue";
import UploadModal from "./UploadModal.vue";
import ScaleModal from "./ScaleModal.vue";
export default {
  name: "PhotoShop",
  components: {
    SidebarPanel,
    HeaderPanel,
    UploadModal,
    ScaleModal,
  },
  data() {
    return {
      canvas: null,
      ctx: null,
      canvasHelper: null,
      ctxHelper: null,
      resl: null,
      x: 0,
      y: 0,
      showUploadModal: false,
      showScaleModal: false,
      showData: false,
      height: 0,
      width: 0,
      nowH: null,
      nowW: null,
    };
  },
  mounted() {
    this.canvas = this.$refs["drawing"];
    this.ctx = this.canvas.getContext("2d", { willReadFrequently: true });
    this.canvasHelper = this.$refs["canvasHelper"];
    this.ctxHelper = this.canvasHelper.getContext("2d", { willReadFrequently: true });
  },
  methods: {
    draw() {
      this.height = this.$refs.modal.result.height;
      this.width = this.$refs.modal.result.width;
      if (
        this.height > this.canvas.height - 100 ||
        this.width > this.canvas.width - 100
      ) {
        let scale_factor = Math.min(
          this.canvas.width / this.width,
          this.canvas.height / this.height
        );
        let newWidth = this.width * scale_factor - 100;
        let newHeight = this.height * scale_factor - 100;
        this.nowH = Math.round(newHeight);
        this.nowW = Math.round(newWidth);
        let x = this.canvas.width / 2 - newWidth / 2;
        let y = this.canvas.height / 2 - newHeight / 2;
        this.ctx.drawImage(this.$refs.modal.result, x, y, newWidth, newHeight);
      } else {
        let x = this.canvas.width / 2 - this.height / 2;
        let y = this.canvas.height / 2 - this.width / 2;
        this.nowH = Math.round(this.height);
        this.nowW = Math.round(this.width);
        this.ctx.drawImage(
          this.$refs.modal.result,
          x,
          y,
          this.width,
          this.height
        );
      }
    },
    showCoordinates(e) {
      this.x = e.offsetX;
      this.y = e.offsetY;
    },
    save() {
      if (this.$refs.modal.result != null) {
        this.resl = this.ctx.getImageData(this.x, this.y, 1, 1).data;
      }
    },
    changeUploadModal() {
      this.showUploadModal = !this.showUploadModal;
    },
    changeScaleModal() {
      this.showScaleModal = !this.showScaleModal;
    },
    showDisplay() {
      this.showData = true;
    },
    closeDisplay() {
      this.showData = false;
    },
    clear() {
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
      this.resl = null;
    },
    saveImage() {
      this.canvasHelper.width = this.width
      this.canvasHelper.height = this.height
      this.ctxHelper.drawImage(this.$refs.modal.result, 0, 0, this.width, this.height)
      const imageDataURL = this.canvasHelper.toDataURL("image/png");
      const link = document.createElement("a");
      link.href = imageDataURL;
      link.download = "my_image.png";
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    },
    resize() {
      let dx = this.canvas.width / 2 - this.nowW / 2;
      let dy = this.canvas.height / 2 - this.nowH / 2;
      const img = this.ctx.getImageData(dx, dy, this.nowW, this.nowH)
      const originalWidth = this.nowW;
      const originalHeight = this.nowH;
      const newHeight = this.$refs.scaleModal.outputH
      const newWidth = this.$refs.scaleModal.outputW
      const scaleX = originalWidth / newWidth;
      const scaleY = originalHeight / newHeight;
      const newData = new Uint8ClampedArray(newWidth * newHeight * 4);

      for (let y = 0; y < newHeight; y++) {
        for (let x = 0; x < newWidth; x++) {
          const px = Math.floor(x * scaleX);
          const py = Math.floor(y * scaleY);
          const index = (y * newWidth + x) * 4;
          const originalIndex = (py * originalWidth + px) * 4;

          newData[index] = img.data[originalIndex];
          newData[index + 1] = img.data[originalIndex + 1];
          newData[index + 2] = img.data[originalIndex + 2];
          newData[index + 3] = img.data[originalIndex + 3];
        }
      }
      let nx = this.canvas.width / 2 - newWidth / 2;
      let ny = this.canvas.height / 2 - newHeight / 2;
      let image = new ImageData(newData, newWidth, newHeight);
      this.clear();
      this.ctx.putImageData(image, nx, ny)
      this.height = newHeight
      this.width = newWidth
      this.nowH = newHeight
      this.nowW = newWidth
    },
    scaleImage() {
      let scalePers = this.$refs.sidebar.scale / 100;
      if (
        this.height > this.canvas.height - 100 ||
        this.width > this.canvas.width - 100
      ) {
        let scale_factor = Math.min(
          this.canvas.width / this.width,
          this.canvas.height / this.height
        );
        let newWidth = (this.width * scale_factor - 100) * scalePers;
        let newHeight = (this.height * scale_factor - 100) * scalePers;
        this.nowH = Math.round(newHeight);
        this.nowW = Math.round(newWidth);
        let x = this.canvas.width / 2 - newWidth / 2;
        let y = this.canvas.height / 2 - newHeight / 2;
        this.clear();
        this.ctx.drawImage(this.$refs.modal.result, x, y, newWidth, newHeight);
      } else {
        let newWidth = this.width * scalePers;
        let newHeight = this.height * scalePers;
        this.nowH = Math.round(newHeight);
        this.nowW = Math.round(newWidth);
        let x = this.canvas.width / 2 - newHeight / 2;
        let y = this.canvas.height / 2 - newWidth / 2;
        this.clear();
        this.ctx.drawImage(this.$refs.modal.result, x, y, newWidth, newHeight);
      }
    },
  },
  computed: {},
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

.drawing {
  height: 642px;
  background-image: url("@/assets/1674303626_catherineasquithgallery-com-p-fon-serii-kvadratiki-foto-22.jpg");
  background-size: 20%;
  background-repeat: repeat;
  border: 1px solid #e0e1dd;
}
</style>
