<template>
    <div class="desktop-icon">
        <img :src="icon" :style="iconStyle" @click="handleClick" />
        <p>{{ text }}</p>
    </div>

  </template>
  
  <script>
  export default {
    props: {
      icon: String,
      iconStyle: Object,
      targetPage: String,
      text: String, 
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
    .desktop-icon {
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      margin: 10px;
    }
    </style>