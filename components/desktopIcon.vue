<template>
    <img :src="icon" :style="iconStyle" @click="handleClick" />
  </template>
  
  <script>
  export default {
    props: {
      icon: String,
      iconStyle: Object,
      targetPage: String, 
    },
    methods: {
      handleClick() {
        this.$emit("icon-click", this.targetPage);
      },
    },
  };
  </script>
    <style scoped>
    img {
      width: 50px;
      height: 50px;
      cursor: pointer;
    }
    </style>