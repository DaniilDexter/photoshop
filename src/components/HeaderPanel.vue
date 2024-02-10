<template>
  <div class="content">
    <button class="button" @click="$emit('show')">Отобразить</button>
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
  },
};
</script>
  
  <style lang="scss" scoped>
.content {
  height: 80px;
  background-color: #1b263b;
  border-bottom: 1px solid black;
  display: flex;
  flex-direction: row;
  align-items: center;
  column-gap: 50px;
  padding: 10px;
}
.button{
  background-color: #778da9;
  border: 1px solid #0d1b2a;
  border-radius: 10px;
  padding: 10px;
  height: fit-content;
}
</style>
  