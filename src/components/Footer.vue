<template>
  <footer id="footer" :class="store.footerBlur ? 'blur' : null">
    <Transition name="fade" mode="out-in">
      <div v-if="!store.playerState || !store.playerLrcShow" class="power">
        
        <span>
          Copyright&nbsp;&copy;
          <span v-if="siteStartDate?.length >= 4" class="site-start">
            {{ siteStartDate.substring(0, 4) }}
            -
          </span>
          {{ fullYear }}
          <a :href="siteUrl">{{ siteAnthor }}</a>
        </span>
        <!-- 以下信息请不要修改哦 -->
        <!-- <span class="hidden">
          &amp;&nbsp;Made&nbsp;by
          <a :href="config.github" target="_blank">
            {{ config.author }}
          </a>
        </span> -->
        <!-- 站点备案 -->
        <a v-if="siteIcp" href="https://beian.miit.gov.cn" target="_blank">
          &nbsp;| &nbsp;
          <img src="https://img.101jc.com/img/icp.png!yuantu" alt="ICP图标" style="width:14px;height:14px;padding-top: 2px;">
          {{ siteIcp }}
        </a>

        <a v-if="siteGONGAN" href="https://beian.mps.gov.cn/#/query/webSearch" target="_blank">
          &nbsp;| &nbsp;
          <img src="https://img.101jc.com/img/gongan.png!yuantu" alt="公安图标" style="width:14px;height:14px;padding-top: 2px;">
          {{ siteGONGAN }}
        </a>
        <span v-if="visitorData" class="hidden">
          <!-- 索引1：最近访客数，索引3：今日访客数，索引5：今日访问量，索引7：昨日访客数，索引9：昨日访问量，索引11：本月访问量，索引13：总访问量 -->
          &nbsp | &nbsp今日访问量 {{ visitorData[5] }}&nbsp | &nbsp总访问量 {{ visitorData[13] }}&nbsp | &nbsp已持续运行 {{ runningDays }} 天&nbsp | &nbsp
        </span>
        <Transition name="slide">

          <span :key="currentTextIndex" class="slide-text">
            {{ getCurrentText }}
          </span>

      </Transition>
      </div>
      <div v-else class="lrc">
        <Transition name="fade" mode="out-in">
          <div class="lrc-all" :key="store.getPlayerLrc">
            <music-one theme="filled" size="18" fill="#efefef" />
            <span class="lrc-text text-hidden" v-html="store.getPlayerLrc" />
            <music-one theme="filled" size="18" fill="#efefef" />
          </div>
        </Transition>
      </div>
    </Transition>
  </footer>

</template>

<script setup>
import { MusicOne } from "@icon-park/vue-next";
import { mainStore } from "@/store";
import config from "@/../package.json";

const store = mainStore();
const fullYear = new Date().getFullYear();

// 加载配置数据
const siteStartDate = ref(new Date(import.meta.env.VITE_SITE_START));
const siteIcp = ref(import.meta.env.VITE_SITE_ICP);
const siteGONGAN = ref(import.meta.env.VITE_SITE_GONGAN);
const siteAnthor = ref(import.meta.env.VITE_SITE_ANTHOR);
const siteUrl = computed(() => {
  const url = import.meta.env.VITE_SITE_URL;
  if (!url) return "https://www.imsyy.top";
  // 判断协议前缀
  if (!url.startsWith("http://") && !url.startsWith("https://")) {
    return "//" + url;
  }
  return url;
});

// 访客数据
const visitorData = ref([]);
// 获取访客数据的方法
const getVisitorData = async () => {
  try {
    const timestamp = new Date().getTime();
    const response = await fetch(`https://v6-widget.51.la/v6/3HjGAq3ibCpbwWfo/quote.js?_=${timestamp}`);
    const data = await response.text();
    const num = data.match(/<span>.*?<\/span>/g);
    const parsedData = num.map(el => el.replace(/<\/?span>/g, ""));
    visitorData.value = parsedData;
  } catch (error) {
    console.error("Error fetching visitor data:", error);
  }
};

// 在组件挂载时获取访客数据
onMounted(() => {
  getVisitorData();
});

// 计算运行天数
const runningDays = computed(() => {
  const oneDay = 24 * 60 * 60 * 1000; // hours*minutes*seconds*milliseconds
  const today = new Date();
  const diffDays = Math.round(Math.abs((today - siteStartDate.value) / oneDay));
  return diffDays;
});

// 轮播文本内容
const texts = [
  "本站已开启CDN缓存，[Ctrl + F5] 可查看最新动态",
  // "点击鼠标右键可以打开或关闭留言板",
  // "有任何问题或建议，请留言告诉我"
];
// 当前显示的文本索引
const currentTextIndex = ref(0);
// 计算属性，用于获取当前应显示的文本
const getCurrentText = computed(() => {
  return texts[currentTextIndex.value % texts.length];
});
// 设置轮播定时器
let intervalId = null;
onMounted(() => {
  intervalId = setInterval(() => {
    currentTextIndex.value = (currentTextIndex.value + 1) % texts.length;
  }, 5000); // 每5秒切换一次
});
onUnmounted(() => {
  clearInterval(intervalId);
});
// 动画钩子函数
const beforeEnter = (el) => {
  el.style.opacity = 0;
  el.style.transform = 'translateY(20px)';
};
const enter = (el, done) => {
  el.style.opacity = 0;
  el.style.transform = 'translateY(20px)';
  Vue.nextTick(() => {
    setTimeout(() => {
      el.style.opacity = 1;
      el.style.transform = 'translateY(0)';
      done();
    }, 500); // 在旧文本完全消失后延迟50ms
  });
};

const beforeLeave = (el) => {
  el.style.opacity = 1;
  el.style.transform = 'translateY(0)';
};
const leave = (el, done) => {
  el.style.opacity = 1;
  el.style.transform = 'translateY(0)';
  Vue.nextTick(() => {
    setTimeout(() => {
      el.style.opacity = 0;
      el.style.transform = 'translateY(-20px)';
      done();
    }, 0); // 在旧文本完全消失后延迟50ms
  });
};

</script>

<style lang="scss" scoped>
#footer {
  width: 100%;
  position: absolute;
  bottom: 0;
  left: 0;
  height: 46px;
  line-height: 46px;
  text-align: center;
  z-index: 0;
  font-size: 14px;
  .power {
    animation: fade 0.3s;
  }
  .lrc {
    padding: 0 20px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    .lrc-all {
      width: 98%;
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      .lrc-text {
        margin: 0 8px;
      }
      .i-icon {
        width: 18px;
        height: 18px;
        display: inherit;
      }
    }
  }
  &.blur {
    backdrop-filter: blur(10px);
    background: rgb(0 0 0 / 25%);
    font-size: 14px;
  }
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.15s ease-in-out;
  }
  @media (max-width: 720px) {
    font-size: 0.85rem;
    &.blur {
      font-size: 0.85rem;
    }
  }
  @media (max-width: 480px) {
    .hidden {
      display: none;
    }
  }
}

/* 添加滑动过渡效果 */
.slide-enter-active,
.slide-leave-active {
  transition: all 0.5s ease;
}
.slide-text {
    width: 200px;
    white-space: nowrap; /* Prevent text wrapping */
    overflow: hidden; /* Hide overflowing text */
  }
.slide-enter-from {
  transform: translateY(20px);
  opacity: 0;
}
.slide-leave-to {
  transform: translateY(-20px);
  opacity: 0;
}
</style>
