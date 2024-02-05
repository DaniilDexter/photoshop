<template>
  <PhotoShop/>
</template>

<script>
import PhotoShop from './components/PhotoShop.vue'

export default {
  name: 'App',
  components: {
    PhotoShop
  }
}
</script>

<style>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-color: #0d1b2a;
  height: 100vh;
}
</style>

<!-- 
Цветовая палитра 
Основной - 0d1b2a
Дополнительный - 1b263b
Акцентный - e0e1dd 
-->