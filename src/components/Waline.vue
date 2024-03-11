<template>
    <div id="waline"></div>
  </template>
  
  <script setup>
  import { onMounted } from 'vue';
  import { init } from '@waline/client';
  import '@waline/client/style';
  
  const props = defineProps({
    serverURL: {
      type: String,
      required: true
    },
    // 可以在这里继续定义更多的props，以允许从父组件传递配置
  });
  
  onMounted(() => {
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
    ],
    });
  });
  </script>
  
  <style scoped>
  /* 如果需要，可以在这里添加样式 */
  </style>
  