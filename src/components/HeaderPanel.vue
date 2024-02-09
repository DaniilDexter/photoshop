<template>
  <div class="content">
    <button @click="$emit('download')">Отобразить</button>
    <input type="file" ref="input" @change="savenew" />
    <button @click="$emit('show')">Отобразить</button>
  </div>
</template>
  
  <script>
export default {
  name: "HeaderPanel",
  data() {
    return {
      new: null,
    };
  },

  methods: {
    savenew(e){
      let img = e.target.files[0]
      this.new = new Image
      this.new.src = URL.createObjectURL(img)
    }
  },
};
</script>
  
  <style lang="scss" scoped>
.content {
  height: 80px;
  background-color: #1b263b;
  border-bottom: 1px solid black;
}
</style>
  