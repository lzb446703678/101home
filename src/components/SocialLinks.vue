<template>
  <!-- 社交链接 -->
  <div class="social">
    <div class="link">
      <a
        v-for="item in socialLinks"
        :key="item.name"
        :href="item.url"
        target="_blank"
        @mouseenter="socialTip = item.tip"
        @mouseleave="socialTip = ''"
      >
        <img class="icon" :src="item.icon" height="24" />
      </a>
      <a  href="javascript:void(0)" @click="showWechatImage" @mouseenter="socialTip = '微信'">
        <img class="icon" src="https://img.101jc.com/img/wechat-dark.png" height="24" />
        <div class="iconTip"></div>
      </a>
      <a href="javascript:void(0)" @click="toggleMessageBox" @mouseenter="socialTip = '留言'">
      <img class="icon-ly" src="https://img.101jc.com/img/liuyan-dark.png" height="24" />
      <div class="iconTip"></div>
    </a>
    </div>
    <span class="tip" v-show="!isWechatImageVisible">{{ socialTip }}</span>
    <div v-show="isWechatImageVisible" class="wechat-popup" @click.stop>
      <img src="https://img.101jc.com/img/wechat.jpg" height="271" width="200" @click.stop />
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import socialLinks from "@/assets/socialLinks.json";
import { mainStore } from "@/store";

// 获取mainStore的实例
const store = mainStore();
// 触发自定义事件来通知父组件切换留言框的状态
const toggleMessageBox = () => {
  store.boxOpenState = !store.boxOpenState;
};


// 社交链接提示
const socialTip = ref("");
// 控制微信图片的显示
const isWechatImageVisible = ref(false);
// 显示微信图片的函数
const showWechatImage = () => {
  isWechatImageVisible.value = true;
};
// 隐藏微信图片的函数
const hideWechatImage = () => {
  isWechatImageVisible.value = false;
};
// 监听文档的点击事件
const handleDocumentClick = (event) => {
  // 检查点击的目标是否是微信图片或者微信图片的弹出层
  if (event.target.className !== 'wechat-popup' && event.target.tagName !== 'IMG') {
    hideWechatImage();
  }
};
// 在组件挂载时添加事件监听器
onMounted(() => {
  document.addEventListener('click', handleDocumentClick);
});
// 在组件卸载时移除事件监听器
onUnmounted(() => {
  document.removeEventListener('click', handleDocumentClick);
});
</script>

<style lang="scss" scoped>
.social {
  margin-top: 1rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 460px;
  width: 100%;
  height: 42px;
  background-color: transparent;
  border-radius: 6px;
  backdrop-filter: blur(0);
  animation: fade 0.5s;
  transition:
    background-color 0.3s,
    backdrop-filter 0.3s;
  @media (max-width: 840px) {
    max-width: 100%;
    justify-content: center;
    .link {
      justify-content: space-evenly !important;
      width: 90%;
    }
    .tip {
      display: none !important;
    }
  }

  .wechat-popup {
  position: fixed;
  bottom: 0px; 
  right: 10px;
  z-index: 0;
}


  .link {
    display: flex;
    align-items: center;
    justify-content: center;
    a {
      display: inherit;
      .icon {
        margin: 0 12px;
        transition: transform 0.3s;
        &:hover {
          transform: scale(1.1);
        }
        &:active {
          transform: scale(1);
        }
      }
      .icon-ly {
        margin: 0 12px;
        transition: transform 0.3s;
        &:hover {
          transform: scale(1.1);
        }
        &:active {
          transform: scale(1);
        }
      }
    }
  }
  .tip {
    display: none;
    margin-right: 12px;
    animation: fade 0.5s;
  }
  @media screen and (max-width: 821px) {
    .icon-ly {
        display: none !important;
        margin: 0 12px;
        transition: transform 0.3s;
        &:hover {
          transform: scale(1.1);
        }
        &:active {
          transform: scale(1);
        }
      }
    }
  @media (min-width: 768px) {
    &:hover {
      background-color: #00000040;
      backdrop-filter: blur(5px);
      .tip {
        display: block;
      }
    }
  }
}
</style>
