<template>
  <div class="modal">
    <div class="modal__container header">
      <p class="header__text">Определение цвета пикселя</p>
      <button class="header__button" @click="$emit('show')">X</button>
    </div>
    <div class="modal__line">
      <div class="modal__element matrix">
        <div class="matrix__line">
          <input
            class="matrix__input"
            type="number"
            @change="changeX1"
            v-model="x1"
            :disabled="prewiev"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX2"
            v-model="x2"
            :disabled="prewiev"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX3"
            v-model="x3"
            :disabled="prewiev"
          />
        </div>
        <div class="matrix__line">
          <input
            class="matrix__input"
            type="number"
            @change="changeX4"
            v-model="x4"
            :disabled="prewiev"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX5"
            v-model="x5"
            :disabled="prewiev"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX6"
            v-model="x6"
            :disabled="prewiev"
          />
        </div>
        <div class="matrix__line">
          <input
            class="matrix__input"
            type="number"
            @change="changeX7"
            v-model="x7"
            :disabled="prewiev"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX8"
            v-model="x8"
            :disabled="prewiev"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX9"
            v-model="x9"
            :disabled="prewiev"
          />
        </div>
      </div>
      <div class="modal__element type">
        <label class="modal__label">
          <input type="radio" name="" value="same" v-model="type"  :disabled="prewiev"/>
          Тождественное отображение
        </label>
        <label class="modal__label">
          <input type="radio" name="" value="sharp" v-model="type"  :disabled="prewiev"/>
          Повышение резкости
        </label>
        <label class="modal__label">
          <input type="radio" name="" value="gaus" v-model="type"  :disabled="prewiev"/>
          Фильтр Гаусса (3 на 3)
        </label>
        <label class="modal__label">
          <input type="radio" name="" value="rect" v-model="type"  :disabled="prewiev"/>
          Прямоугольное размытие
        </label>
      </div>
    </div>
    <div class="modal__line">
      <label>
        <input type="checkbox" v-model="prewiev" @change="applyPrewiev" />
        Включить предпросмотр
      </label>
    </div>
    <div class="modal__line">
      <div class="modal__element">
        <button class="modal__button" @click="filter(), $emit('apply')">Применить</button>
      </div>
      <div class="modal__element">
        <button class="modal__button" @click="reset" :disabled="prewiev">Сбросить</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "FilteringModal",
  props: {
    dx: Number,
    dy: Number,
    nowW: Number,
    nowH: Number,
    ctxRef: CanvasRenderingContext2D,
    startImage: Object,
  },
  data() {
    return {
      prewiev: false,
      type: "same",
      x1: 0,
      x2: 0,
      x3: 0,
      x4: 0,
      x5: 1,
      x6: 0,
      x7: 0,
      x8: 0,
      x9: 0,
      kernel: [
        [0, 0, 0],
        [0, 1, 0],
        [0, 0, 0],
      ],
    };
  },
  methods: {
    changeX1() {
      this.kernel[0][0] = this.x1;
      this.type = "";
    },
    changeX2() {
      this.kernel[0][1] = this.x2;
      this.type = "";
    },
    changeX3() {
      this.kernel[0][2] = this.x3;
      this.type = "";
    },
    changeX4() {
      this.kernel[1][0] = this.x4;
      this.type = "";
    },
    changeX5() {
      this.kernel[1][1] = this.x5;
      this.type = "";
    },
    changeX6() {
      this.kernel[1][2] = this.x6;
      this.type = "";
    },
    changeX7() {
      this.kernel[2][0] = this.x7;
      this.type = "";
    },
    changeX8() {
      this.kernel[2][1] = this.x8;
      this.type = "";
    },
    changeX9() {
      this.kernel[2][2] = this.x9;
      this.type = "";
    },
    reset(){
      this.type = "same"
    },
    applyPrewiev() {
      if (this.prewiev) {
        this.filter();
      } else {
        this.ctxRef.drawImage(
          this.startImage,
          this.dx,
          this.dy,
          this.nowW,
          this.nowH
        );
      }
    },
    filter() {
      this.ctxRef.drawImage(
          this.startImage,
          this.dx,
          this.dy,
          this.nowW,
          this.nowH
        );
      const imageData = this.ctxRef.getImageData(this.dx, this.dy, this.nowW, this.nowH);
      const newData = new Uint8ClampedArray(imageData.data.length);

      const paddedData = this.padImageData(
        imageData.data,
        imageData.width,
        imageData.height
      );

      for (let y = 0; y < imageData.height; y++) {
        for (let x = 0; x < imageData.width; x++) {
          for (let c = 0; c < 4; c++) {
            const outputIndex = (y * imageData.width + x) * 4 + c;
            let sum = 0;
            let kernelSum = 0;
            for (let ky = 0; ky < 3; ky++) {
              for (let kx = 0; kx < 3; kx++) {
                const inputIndex =
                  ((y + ky) * (imageData.width + 2) + (x + kx)) * 4 + c;
                sum += paddedData[inputIndex] * this.kernel[ky][kx];
                kernelSum += this.kernel[ky][kx];
              }
            }
            newData[outputIndex] = sum / kernelSum;
          }
        }
      }

      imageData.data.set(newData);
      this.ctxRef.putImageData(imageData, this.dx, this.dy);
    },

    padImageData(data, width, height) {
      const paddedWidth = width + 2;
      const paddedHeight = height + 2;
      const paddedData = new Uint8ClampedArray(paddedWidth * paddedHeight * 4);

      for (let y = 0; y < height; y++) {
        for (let x = 0; x < width; x++) {
          const inputIndex = (y * width + x) * 4;
          const outputIndex = ((y + 1) * paddedWidth + x + 1) * 4;
          paddedData.set(
            data.subarray(inputIndex, inputIndex + 4),
            outputIndex
          );
        }
      }

      for (let y = 0; y < paddedHeight; y++) {
        for (let x = 0; x < paddedWidth; x++) {
          const outputIndex = (y * paddedWidth + x) * 4;
          if (
            x === 0 ||
            x === paddedWidth - 1 ||
            y === 0 ||
            y === paddedHeight - 1
          ) {
            const nearestX = Math.max(1, Math.min(x, paddedWidth - 2));
            const nearestY = Math.max(1, Math.min(y, paddedHeight - 2));
            const nearestIndex = (nearestY * paddedWidth + nearestX) * 4;
            paddedData.set(
              paddedData.subarray(nearestIndex, nearestIndex + 4),
              outputIndex
            );
          }
        }
      }
      return paddedData;
    },
  },
  watch: {
    type: function (value) {
      if (value == "same") {
        this.kernel = [
          [0, 0, 0],
          [0, 1, 0],
          [0, 0, 0],
        ];
      }
      if (value == "sharp") {
        this.kernel = [
          [-1, -1, -1],
          [-1, 9, -1],
          [-1, -1, -1],
        ];
      }
      if (value == "gaus") {
        this.kernel = [
          [1, 2, 1],
          [2, 4, 2],
          [1, 2, 1],
        ];
      }
      if (value == "rect") {
        this.kernel = [
          [1, 1, 1],
          [1, 1, 1],
          [1, 1, 1],
        ];
      }
    },
    kernel: function (value) {
      this.x1 = value[0][0];
      this.x2 = value[0][1];
      this.x3 = value[0][2];
      this.x4 = value[1][0];
      this.x5 = value[1][1];
      this.x6 = value[1][2];
      this.x7 = value[2][0];
      this.x8 = value[2][1];
      this.x9 = value[2][2];
    },
    // x1: function(value){
    //     this.kernel[0][0] = value
    //     this.type = ""
    // },
    // x2: function(value){
    //     this.kernel[0][1] = value
    // },
    // x3: function(value){
    //     this.kernel[0][2] = value
    // },
    // x4: function(value){
    //     this.kernel[1][0] = value
    // },
    // x5: function(value){
    //     this.kernel[1][1] = value
    // },
    // x6: function(value){
    //     this.kernel[1][2] = value
    // },
    // x7: function(value){
    //     this.kernel[2][0] = value
    // },
    // x8: function(value){
    //     this.kernel[2][1] = value
    // },
    // x9: function(value){
    //     this.kernel[2][2] = value
    // },
  },
};
</script>
<style lang="scss" scoped>
.modal {
  position: absolute;
  top: 12px;
  right: 12px;
  background-color: #778da9;
  width: 25vw;
  padding: 15px;
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  row-gap: 20px;

  &__container {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
  }

  &__line {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }

  &__element {
    width: 50%;
    text-align: center;
  }

  &__button{
    width: 90px;
    border: 1px solid black;
    padding: 4px;
    border-radius: 5px;
  }

  &__label {
    display: flex;
    flex-direction: row;
    column-gap: 20px;
    align-items: center;
  }

  &__input {
    width: 30%;
    border-radius: 6px;
    padding: 3px;
    border: 1px solid black;
  }
}

.type {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.matrix {
  display: flex;
  flex-direction: column;
  row-gap: 5px;

  &__line {
    display: flex;
    flex-direction: row;
    column-gap: 5px;
  }

  &__input {
    width: 45px;
    padding: 3px;
  }
}

.header {
  &__text {
    font-size: 20px;
  }
  &__button {
    background: none;
    border: none;
    color: red;
    font-size: 24px;
    cursor: pointer;
  }
}
</style>