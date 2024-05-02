<template>
  <div class="modal-wrapper">
    <div class="modal">
      <div class="modal__header header">
        <p class="header__text">Выберите фотографию</p>
        <button class="header__button" @click="$emit('show')">X</button>
      </div>
      <label>
        URL адрес:
        <input class="input" type="text" @change="savenewUrl" v-model="url" />
      </label>
      <label>
        Файл:
        <input
          class="input"
          type="file"
          ref="inputFile"
          @change="savenewFile"
        />
      </label>
      <button
        class="button"
        @click="saveResult(), $emit('show'), $emit('download')"
      >
        Отобразить
      </button>
    </div>
  </div>
</template>
    
    <script>
export default {
  name: "UploadModal",
  data() {
    return {
      Url: null,
      File: null,
      url: null,
      result: null,
    };
  },
  mounted() {
  },
  methods: {
    async savenewUrl() {
      fetch(this.url)
        .then((response) => response.blob())
        .then((blob) => {
          this.File = new Image();
          this.File.src = URL.createObjectURL(blob);
        });
    },
    savenewFile(e) {
      let img = e.target.files[0];
      this.File = new Image();
      this.File.src = URL.createObjectURL(img);
    },
    saveResult() {
      if (this.Url != null && this.File != null) {
        this.result = this.File;
      } else if (this.Url != null && this.File == null) {
        this.result = this.Url;
      } else if (this.Url == null && this.File != null) {
        this.result = this.File;
      }
      this.Url = null;
      this.url = null;
      this.$refs.inputFile.value = null;
      this.File = null;
    },
  },
};
</script>
    
<style lang="scss" scoped>
.modal-wrapper {
  height: 100vh;
  width: 100vw;
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
  height: 30vh;
  width: 30vw;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid black;

  &__header {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
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
    