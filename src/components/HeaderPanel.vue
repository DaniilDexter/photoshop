<template>
  <div class="content">
    <button class="button" @click="$emit('show')" title="Upload your image!">
      <img class="button__img" src="@/assets/download-photo-svgrepo-com.svg" alt="">
    </button>
    <button class="button" :class="{ active: isPippet}" @click="$emit('show')" title="Color picker">
      <img class="button__img" src="@/assets/pipette.svg" alt="">
    </button>
    <button class="button" :class="{ active: isGrab}" @click="$emit('grab')" title="Grab your image">
      <img class="button__img" src="@/assets/hand-grab.svg" alt="">
    </button>
    <button class="button" @click="$emit('scale')" title="Resize your image">
      <img class="button__img" src="@/assets/scale-up.svg" alt="">
    </button>
    <button class="button" @click="$emit('save')" title="Save your image">
      <img class="button__img" src="@/assets/save.svg" alt="">
    </button>
    <button class="button" @click="$emit('clear')" title="Clear">
      <img class="button__img" src="@/assets/delete-2-svgrepo-com.svg" alt="">
    </button>
  </div>
</template>
  
  <script>
export default {
  name: "HeaderPanel",
  props: {
    state: String,
  },
  data() {
    return {
      new: null,
    };
  },

  methods: {
  },
  computed:{
    isPippet(){
      if (this.state == 'pippet'){
        return true
      } else {
        return false
      }
    },
    isGrab(){
      if (this.state == 'grab'){
        return true
      } else {
        return false
      }
    }
  }
};
</script>
  
  <style lang="scss" scoped>
.content {
  height: 80px;
  background-color: #1b263b;
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
  padding: 7px;
  height: fit-content;
  margin-left: 20px;

  &__img{
    height: 35px;
  }

  &:last-child{
    margin-left: auto;
    margin-right: 20px;
  }

  &.active{
    background-color: #1b263b;
  }
}
</style>
  