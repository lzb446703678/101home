<template>
  <div :class="store.mobileOpenState ? 'left hidden' : 'left'">
    <Message />
    <div class="hidden">
      <div class="line">
  <img src="https://img.101jc.com/img/Compass-dark.svg" alt="Link Icon" style="width: 20px; height: 20px;">
  <span class="title" style="color: #000000;">更新日志</span>
</div>
    
    <div class="cards-timeline" style="max-width: 465px; display: flex;">
      <Timeline />
      <!-- <img class="hidden" src="https://img.101jc.com/img/gongzhonghao6.png" alt="GongZhongHao" style="height: 220px; margin-left:auto;"> -->
    </div>
  </div>
    <SocialLinks />
  </div>
</template>

<script setup>
import { mainStore } from "@/store";
import Message from "@/components/Message.vue";
import SocialLinks from "@/components/SocialLinks.vue";
import Timeline from "@/views/Main/Timeline.vue";
import { Icon } from "@vicons/utils";
// 可前往 https://www.xicons.org 自行挑选并在此处引入
import { Link, Blog, CompactDisc, Cloud, Compass, Book, Fire, LaptopCode,  Toolbox,UserCircle,LocationArrow,PhotoVideo, UserFriends} from "@vicons/fa"; // 注意使用正确的类别
const store = mainStore();

// 网站链接图标
const siteIcon = {
  Blog,
  Cloud,
  CompactDisc,
  Compass,
  Book,
  Fire,
  LaptopCode,
  Toolbox,
  UserCircle,
  LocationArrow,
  PhotoVideo,
  UserFriends,
};
</script>

<style lang="scss" scoped>
.left {
  // flex: 1 0 0%;
  width: 50%;
  margin-right: 0px;
  transform: translateY(-83px);
  &.hidden {
    display: none;
  }
  .line {
    margin: 2rem 0.25rem 1rem;
    font-size: 1.1rem;
    display: flex;
    align-items: center;
    animation: fade 0.5s;
    .title {
      margin-left: 8px;
      font-size: 1.15rem;
      text-shadow: 0 0 5px #00000050;
    }
  }
  @media (max-width: 720px) {
    margin-right: 0;
    width: 100%;
    transform: translateY(-20px);
  }
  @media (max-width: 768px) {
    .hidden {
      display: none;
    }
  }
}
</style>
