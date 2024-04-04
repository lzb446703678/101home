<template>
  <div id="waline"></div>
</template>

<script setup>
import { onMounted } from 'vue';
import { init } from '@waline/client';

const props = defineProps({
  serverURL: {
    type: String,
    required: true
  },
  // 可以在这里继续定义更多的props，以允许从父组件传递配置
});

onMounted(() => {
  // 动态添加Waline的CSS文件
  const link = document.createElement('link');
  link.rel = 'stylesheet';
  link.href = 'https://img.101jc.com/img/waline.css';
  document.head.appendChild(link);

  // 初始化Waline
  init({
    el: '#waline',
    serverURL: "https://vercel-waline.101jc.com/",
    // 可以继续添加更多的Waline配置项
    pageview: true, // 启用页面浏览量统计
    comment: true, // 评论数统计
    dark: true,
    emoji: [
      'https://unpkg.com/@waline/emojis@1.0.1/weibo',
      'https://unpkg.com/@waline/emojis@1.0.1/alus',
      'https://unpkg.com/@waline/emojis@1.2.0/bilibili',
      'https://unpkg.com/@waline/emojis@1.2.0/bmoji',
      'https://unpkg.com/@waline/emojis@1.2.0/qq',
      'https://unpkg.com/@waline/emojis@1.2.0/tieba',
      'https://unpkg.com/@waline/emojis@1.2.0/tw-emoji',
    ],
    reaction: [
    'https://img.101jc.com/img/sun_with_face_3d.png',
    'https://img.101jc.com/img/smiling_face_with_smiling_eyes_3d.png',
    'https://img.101jc.com/img/yawning_face_3d.png',
    'https://img.101jc.com/img/pensive_face_3d.png',
    'https://img.101jc.com/img/confounded_face_3d.png',
  ],
  locale: {
    reactionTitle: "请不吝留下您对本站的评价",
    reaction0: "非常喜欢",
    reaction1: "喜欢",
    reaction2: "一般",
    reaction3: "不喜欢",
    reaction4: "非常不喜欢",
    placeholder: "期待你的留言！",
    sofa: '来发评论吧~',
  },
  });
});
</script>

<style scoped>
/* 这里可以覆盖Waline的CSS变量 */
</style>
