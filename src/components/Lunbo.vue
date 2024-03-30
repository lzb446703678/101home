<template>
  <div class="text-carousel">
    <div class="carousel-inner">
      <span class="text-and-cursor">
        {{ animatedText }}<span class="cursor">|</span> <!-- 模拟光标 -->
      </span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      texts: [
        "Hi ! 我是一名兴趣广泛的“全干”检测工程师，欢迎来到我的主页",
        "本站已开启CDN缓存，[Ctrl + F5] 可查看最新动态",
        "点击鼠标右键可以打开或关闭留言板",
        "有任何问题或建议，请留言告诉我",
        "知识库目前初步搭建了框架，尚未更新具体内容，后续会从博客中择优整理录入",
      ],
      currentIndex: 0,
      animatedText: '', // 用于逐字显示的文本
      typingSpeed: 150, // 打字速度，单位为毫秒
    };
  },
  methods: {
    typeText() {
      const currentText = this.texts[this.currentIndex];
      let charIndex = 0;
      this.animatedText = ''; // 重置animatedText

      const typeChar = () => {
        if (charIndex < currentText.length) {
          this.animatedText += currentText[charIndex++];
          setTimeout(typeChar, this.typingSpeed);
        } else {
          // 文本打印完成后，准备显示下一段文本
          setTimeout(() => {
            this.nextText();
          }, 2000); // 在这里可以调整文本间的停留时间
        }
      };

      typeChar();
    },
    nextText() {
      this.currentIndex = (this.currentIndex + 1) % this.texts.length;
      this.typeText(); // 继续打印下一段文本
    }
  },
  mounted() {
    this.typeText(); // 组件挂载后开始打字效果
  }
};
</script>

<style>
.carousel-inner {
  display: flex;
  justify-content: flex-start; /* 确保内容从左侧开始 */
  align-items: center;
  height: 50px; /* 适应动画的高度 */
  font-size: 16px;
  color: #333;
  text-align: left; /* 确保文本左对齐 */
  padding: 0; /* 根据需要调整内边距 */
  margin: 0; /* 根据需要调整外边距 */
}


.text-and-cursor {
  white-space: pre-wrap; /* 保持空格和换行 */
}

.text-carousel .cursor {
  animation: blink 1s step-start infinite;
  font-size: 22px;
  color: #38BDF8;
}

@keyframes blink {
  50% {
    opacity: 0;
  }
}
</style>
