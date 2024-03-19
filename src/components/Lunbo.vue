<template>
    <div class="text-carousel">
      <div class="carousel-inner">
        <span v-if="showText" :key="currentIndex" v-text="texts[currentIndex]" />
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        texts: [
          "「 本站已开启CDN缓存，[Ctrl + F5] 可查看最新动态 」",
          "「 点击鼠标右键可以打开或关闭留言板 」",
          "「 有任何问题或建议，请留言告诉我 」",
        ],
        currentIndex: 0,
        showText: true,
        rotationInterval: null,
      };
    },
    mounted() {
      this.startRotation();
    },
    beforeUnmount() {
      clearInterval(this.rotationInterval);
    },
    methods: {
      startRotation() {
        this.rotationInterval = setInterval(() => {
          this.showText = false;
          setTimeout(() => {
            this.currentIndex = (this.currentIndex + 1) % this.texts.length;
            this.showText = true;
          }, 200);
        }, 5000);
      },
    },
  };
  </script>
  
  <style>
  .text-carousel {
    width: 100%;
    overflow: hidden;
  }
  
  .carousel-inner {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 20px;
    font-size: 14px;
    color: #333;
  }
  
  .carousel-inner span {
    opacity: 0;
    transform: translateY(20px);
    animation: slide-up 0.5s ease-in-out forwards;
  }
  
  @keyframes slide-up {
    0% {
      opacity: 0;
      transform: translateY(-20px);
    }
    100% {
      opacity: 1;
      transform: translateY(0);
    }
  }
  </style>
  