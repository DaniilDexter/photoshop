<template>
    <div class="content">
      <button @click="$emit('download')">Отобразить</button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'HeaderPanel',
  }
  </script>
  
  <style lang="scss" scoped>
  
.content{
  height: 80px;
  background-color: #1b263b;
  border-bottom: 1px solid black;
}

  </style>
  