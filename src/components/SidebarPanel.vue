<template>
  <div class="content">
    <p class="font">x: {{ x }}px, y: {{ y }}px</p>
    <template v-if="showData">
      <p class="font">Исходные параметры изображения:</p>
      <div class="params">
        <p class="font">Ширина: {{ width }}px</p> 
        <p class="font">Высота: {{ height }}px</p>
      </div>
    </template>
  </div>
</template>
  
  <script>
export default {
  name: "SidebarPanel",
  props: {
    x: Number,
    y: Number,
    height: Number,
    width: Number,
    showData: Boolean,
  },
  data() {
    return {
      picker: null,
    }
  },
  mounted() {
  },
  methods: {
  },
};
</script>
  
  <style lang="scss" scoped>
.content {
  width: 200px;
  height: 100%;
  border-right: 1px solid black;
  background-color: #1b263b;
  display: flex;
  flex-direction: column;
  row-gap: 20px;
  padding: 10px;
}
.color{
  height: 30px;
  width: 30px;
  border-radius: 15px;
  border: 1px solid #e0e1dd;
  align-self: center;
}
.font{
  color: #e0e1dd;
  &_center{
    align-self: center;
  }
}
.params{
  padding-left: 15px;
  display: flex;
  flex-direction: column;
  row-gap: 7px;
}
</style>
  