<template>
  <div class="wrapper">
    <input type="file" ref="inputUrl"  />
    <input type="file" ref="inputFile"  />
    <button @click="$emit('show')">Отобразить</button>
  </div>
</template>
    
    <script>
export default {
  name: "DownloadModal",
  data() {
    return {
      new: null,
    };
  },

  methods: {
  },
};
</script>
    
    <style lang="scss" scoped>
.wrapper {
  height: 100vh;
  width: 100vw;
  background-color: rgba(0,0,0, 0.4);
  position: absolute;
  left: 0;
  top: 0;
}
</style>
    