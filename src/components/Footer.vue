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
          <a :href="siteUrl" style="color:#38BDF8">{{ siteAnthor }}</a>
        </span>
        <span class="hidden">
          &nbsp;&nbsp; 基于
        <a href="https://github.com/imsyy" target="_blank" style="color:#38BDF8">imsyy</a>
        的开源项目并遵循
        <a href="https://opensource.org/licenses/MIT" target="_blank" style="color:#38BDF8">MIT许可协议</a>
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
          &nbsp; 
          <img src="https://img.101jc.com/img/icp.png" alt="ICP图标" style="width:14px;height:14px;">
          {{ siteIcp }}
        </a>

        <a v-if="siteGONGAN" href="https://beian.mps.gov.cn/#/query/webSearch" target="_blank" class="hidden">
          &nbsp;&nbsp;
          <img src="https://img.101jc.com/img/gongan.png" alt="公安图标" style="width:14px;height:14px;">
          {{ siteGONGAN }}
        </a>
        <a href="https://cloud.tencent.com/" target="_blank" class="hidden">
          &nbsp; &nbsp;由
          <img src="https://img.101jc.com/img/tengxunyun.png" alt="腾讯云图标" style="width:19px;height:14px;">
          腾讯云 提供CDN加速/云存储服务
        </a>
        <span v-if="visitorData" class="hidden">
          <!-- 索引1：最近访客数，索引3：今日访客数，索引5：今日访问量，索引7：昨日访客数，索引9：昨日访问量，索引11：本月访问量，索引13：总访问量 -->
          &nbsp | &nbsp今日访问量 <span style="color:#38BDF8;">{{ visitorData[5] }}</span>&nbsp | &nbsp总访问量 <span style="color:#38BDF8">{{ visitorData[13] }}</span>&nbsp | &nbsp已持续运行 <span style="color:#38BDF8">{{ runningDays }}</span> 天&nbsp | &nbsp
        </span>

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
</style>
