<template>
  <div>
    <div class="tooltipe" @mouseover="showTooltipe" @mouseout="showTooltipe">
      ?
    </div>
    <p class="tooltipe__text" :class="{ tooltipe__text_show: isTooltipe }">
        {{ text }}
    </p>
  </div>
</template>
<script>
export default {
  name: "ToolTipe",
  props: {
    text: String,
  },
  data() {
    return {
      isTooltipe: false,
    };
  },
  methods: {
    showTooltipe() {
      this.isTooltipe = !this.isTooltipe;
    },
  },
};
</script>

<style lang="scss" scoped>
.tooltipe {
  text-decoration: underline;
  cursor: pointer;
  margin-left: 10px;

  &__text {
    position: absolute;
    width: 370px;
    padding: 20px;
    background-color: #0d1b2a;
    text-align: center;
    z-index: 100;
    border-radius: 20px;
    color: #e0e1dd;
    margin-top: 6px;
    display: none;

    &_show {
      display: block;
    }
  }
}
</style>