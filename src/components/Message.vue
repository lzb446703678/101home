<template>
  <!-- 基本信息 -->
  <div class="message">
    <!-- Logo -->
    <div class="logo">
      <img class="logo-img" :src="siteLogo" alt="logo" />
      <!-- <div :class="{ name: true, 'text-hidden': true, long: siteUrl[0].length >= 6 }">
        <span class="bg" style="color: #000000;">{{ siteUrl[0] }}</span>
        <span class="sm" style="color: #000000;">.{{ siteUrl[1] }}</span>
      </div> -->
    </div>
    <div className="SelfDescription">
            <div className="SayWords">
               <div>
                   <h2>Hi!👋</h2>
                   <h2>I'm <span className="highlight">旅者Bin</span></h2>
               </div>
                <h3>一名“全干”检测工程师</h3>
                <!-- <h4>Welcome To My Homepage</h4> -->

                </div>
            </div>
    <!-- 简介 -->
    <!-- <div class="description cards" @click="changeBox">
      <div class="content">
        <Icon size="16">
          <QuoteLeft />
        </Icon>
        <div class="text">
          <p>{{ descriptionText.hello }}</p>
          <div class="lunbo-container">
        <Lunbo />
      </div>
        </div>
        <Icon size="16">
          <QuoteRight />
        </Icon>
      </div>
    </div> -->
  </div>
</template>

<script setup>
import { Icon } from "@vicons/utils";
import { QuoteLeft, QuoteRight } from "@vicons/fa";
import { Error } from "@icon-park/vue-next";
import { mainStore } from "@/store";
import Lunbo from "@/components/Lunbo.vue";
const store = mainStore();

// 主页站点logo
const siteLogo = import.meta.env.VITE_SITE_MAIN_LOGO;
// 站点链接
const siteUrl = computed(() => {
  const url = import.meta.env.VITE_SITE_URL;
  if (!url) return "101jc.com".split(".");
  // 判断协议前缀
  if (url.startsWith("http://") || url.startsWith("https://")) {
    const urlFormat = url.replace(/^(https?:\/\/)/, "");
    return urlFormat.split(".");
  }
  return url.split(".");
});

// 简介区域文字
const descriptionText = reactive({
  hello: import.meta.env.VITE_DESC_HELLO,
  text: import.meta.env.VITE_DESC_TEXT,
});

// 切换右侧功能区
const changeBox = () => {
  if (store.getInnerWidth >= 990) {
    store.boxOpenState = !store.boxOpenState;
  } else {
    ElMessage({
      message: "当前页面宽度不足以开启盒子",
      grouping: true,
      icon: h(Error, {
        theme: "filled",
        fill: "#efefef",
      }),
    });
  }
};

// 监听状态变化
watch(
  () => store.boxOpenState,
  (value) => {
    if (value) {
      descriptionText.hello = import.meta.env.VITE_DESC_HELLO_OTHER;
      descriptionText.text = import.meta.env.VITE_DESC_TEXT_OTHER;
    } else {
      descriptionText.hello = import.meta.env.VITE_DESC_HELLO;
      descriptionText.text = import.meta.env.VITE_DESC_TEXT;
    }
  },
);
</script>

<style lang="scss" scoped>
.message {
  .logo {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center; /* 添加这行来确保子元素水平居中 */
    animation: fade 0.5s;
    max-width: 460px;
    .logo-img {
      border-radius: 50%;
      width: 240px;// 175px
      transform: translate(-120px, 0px);//
      border: 5px solid white;
    }
    .name {
      width: 100%;
      padding-left: 0px;
      transform: translateY(-8px);
      font-family: "Pacifico-Regular";

      .bg {
        font-size: 5rem;
      }

      .sm {
        margin-left: 6px;
        font-size: 2rem;
        @media (min-width: 720px) and (max-width: 789px) {
          display: none;
        }
      }
    }
    @media (max-width: 820px) {
      .logo-img {
        width: 180px;//100px
        transform: translate(0px, -20px);//
      }
      .name {
        height: 128px;
        .bg {
          font-size: 4.5rem;
        }
      }
    }

    @media (max-width: 720px) {
      max-width: 100%;
    }
  }

  .description {
    padding: 1rem;
    margin-top: 0.5rem;
    max-width: 460px;
    animation: fade 0.5s;
    height: 165px;

    .content {
      display: flex;
      justify-content: space-between;

      .text {
        margin: 0.75rem 1rem;
        line-height: 2rem;
        margin-right: auto;

        p {
          &:nth-of-type(1) {
            font-family: "Pacifico-Regular";
          }
        }
      }

      .xicon:nth-of-type(2) {
        align-self: flex-end;
      }
    }
    @media (max-width: 720px) {
      max-width: 100%;
      height: 150px;
      pointer-events: none;
    }
  }
  @media (max-width: 300px) {
    .logo {
      flex-direction: column;
      .logo-img {
        display: none;
      }
      .name {
        margin-left: 0;
        height: auto;
        transform: none;
        text-align: center;
        .bg {
          font-size: 3.5rem;
        }
        .sm {
          font-size: 1.4rem;
        }
      }
    }
    .description {
      margin-top: 2.5rem;
    }
  }
}
.SelfDescription {
  position: relative;
  width: 100%;
  height: 300px;
  display: flex;
  justify-content: flex-start; /* 默认在大屏幕上靠左对齐 */
  align-items: center;

  @media only screen and (max-width: 820px) {
    justify-content: center; /* 在非桌面端屏幕宽度小于1200px时居中对齐 */
    height: 200px;
  }
}

.homeTyped {
  position: absolute;
  bottom: 100px;
  font-weight: 700;
}

.SayWords {
  animation: left-in 1s forwards;
}

.SayWords h2 {
  font-size: 80px;
  font-weight: 900;
  color: #000;
}

.SayWords .highlight {
  color: #0086D1;
}

.SayWords h3 {
  font-size: 40px;
  color: #000;
}



@media only screen and (max-width: 1181px) {
  .SayWords h2 {
    font-size: 60px; /* 较小的屏幕上减小为60px */
  }

  .SayWords h3 {
    font-size: 30px; /* 较小的屏幕上减小为30px */
  }
}



.lunbo-container {
  display: flex;
  justify-content: left;
  align-items: left;
  margin-top: 10px; /* 调整与 logo 的距离 */
  text-align: left; /* 文本居中 */
    
  }
  @media (max-width: 768px) {
    .hidden {
      display: none;
    }
  }
</style>
