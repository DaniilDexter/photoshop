<template>
  <div class="modal-wrapper">
    <div class="modal">
      <div class="modal__container header">
        <p class="header__text">Масштабирование</p>
        <button class="header__button" @click="$emit('show')">X</button>
      </div>
      <label>
        Способ изменения размера
        <select v-model="type" @change="chooseType">
          <option value="pixels">В пикселях</option>
          <option value="persentage">В процентах</option>
        </select>
      </label>
      <div class="modal__container">
        <label>
          Ширина:
          <input
            class="input"
            type="number"
            min="1"
            v-model="width"
            @change="updateWidth"
          />
        </label>
        <label>
          Высота:
          <input
            class="input"
            type="number"
            min="1"
            v-model="height"
            @change="updateHeight"
          />
        </label>
      </div>
      <div class="modal__container">
        <p>Разрешение до: {{ resNow }} MP</p>
        <p>Разрешение после: {{ resAfter }} MP</p>
      </div>
      <label>
        Сохранять заданные пропорции:
        <input type="checkbox" v-model="proprtions" />
      </label>
      <label>
        Алгоритм интерполяции
        <select v-model="interpol">
          <option value="nearest">Ближайшие соседи</option>
        </select>
        <ToolTipe
          text="Nearest Neighbor: Each pixel in the new image is assigned the value of the
      nearest pixel in the original image."
        />
      </label>
      <button class="button" @click="$emit('show'), $emit('resize')">
        Отобразить
      </button>
    </div>
  </div>
</template>
      
<script>
import ToolTipe from "./ToolTipe.vue";

export default {
  name: "ScaleModal",
  props: {
    nowW: Number,
    nowH: Number,
  },
  components: {
    ToolTipe,
  },
  data() {
    return {
      type: "pixels",
      width: this.nowW,
      height: this.nowH,
      outputH: null,
      outputW: null,
      proprtions: false,
      interpol: "nearest",
    };
  },
  watch: {
    nowW: function (value) {
      this.width = value;
    },
    nowH: function (value) {
      this.height = value;
    },
  },
  computed: {
    resNow() {
      return Math.round((this.nowH * this.nowW) / 10000) / 100;
    },
    resAfter() {
      return Math.round((this.outputH * this.outputW) / 10000) / 100;
    },
  },
  methods: {
    chooseType() {
      if (this.type === "persentage") {
        this.width = (this.width / this.nowW) * 100;
        this.height = (this.height / this.nowH) * 100;
      } else {
        this.width = (this.width * this.nowW) / 100;
        this.height = (this.height * this.nowH) / 100;
      }
    },
    updateHeight() {
      if (this.proprtions) {
        let coef = this.nowW / this.nowH;
        this.width = Math.round(this.height * coef);
      }
      if (this.type === "persentage") {
        this.outputH = Math.round((this.height * this.nowH) / 100);
        this.outputW = Math.round((this.width * this.nowW) / 100);
      } else {
        this.outputH = this.height;
        this.outputW = this.width;
      }
    },
    updateWidth() {
      if (this.proprtions) {
        let coef = this.nowW / this.nowH;
        this.height = Math.round(this.width * coef);
      }
      if (this.type === "persentage") {
        this.outputH = Math.round((this.height * this.nowH) / 100);
        this.outputW = Math.round((this.width * this.nowW) / 100);
      } else {
        this.outputH = this.height;
        this.outputW = this.width;
      }
    },
  },
};
</script>
      
<style lang="scss" scoped>
label {
  display: flex;
  flex-direction: row;
  align-items: center;
  column-gap: 5px;
}
.modal-wrapper {
  height: 100vh;
  width: calc(100vw - 17px);
  background-color: rgba(27, 38, 59, 0.9);
  position: absolute;
  left: 0;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background-color: #e0e1dd;
  height: 40vh;
  width: 35vw;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid black;
  opacity: 1;

  &__container {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    position: relative;
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
.button {
  background-color: #778da9;
  border: 1px solid #0d1b2a;
  border-radius: 10px;
  padding: 10px;
  height: fit-content;
}
.input {
  height: 20px;
  border-radius: 5px;
}
</style>
      