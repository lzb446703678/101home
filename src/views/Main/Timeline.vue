<template>
  <div class="left-div">
    <ul id="line">
      <li v-for="(event, index) in events" :key="index">
        <div class="focus"></div>
        <div :class="{'first-event-date': index === 0, 'event-date': index !== 0}">{{ event.date }}</div>
        <div :class="{'first-event-description': index === 0, 'event-description': index !== 0}">{{ event.description }}</div>
      </li>
    </ul>
  </div>
</template>


<script>
export default {
  name: 'TimelineComponent',
  data() {
    return {
      events: [
      { date: '2024-03-12', description: '修改了首页日志的样式，突出显示了最新的日志记录' },
      { date: '2024-03-11', description: '添加了主页留言板功能，添加了微信二维码及留言板按钮' },
      { date: '2024-03-10', description: '制作了动态头像并更新了主页布局' },
      { date: '2024-03-09', description: '搭建IT工具箱' },
      { date: '2024-03-08', description: '搭建站点监测' },
        { date: '2024-03-07', description: '搭建更新日志' },
        { date: '2024-03-06', description: '搭建101知识库' },
        { date: '2024-03-05', description: '学习Github并搭建个人主页' },
        { date: '2024-01-29', description: '发布文章数量达到10篇' },
        { date: '2024-01-23', description: '利用腾讯云COS及Edgeone自建图床' },
        { date: '2024-01-12', description: '使用Halo框架及Hao主题搭建博客' },
        { date: '2023-11-18', description: '公安备案通过' },
        { date: '2023-11-13', description: '公安备案被拒并再次提交' },
        { date: '2023-11-10', description: '首次提交公安备案' },
        { date: '2023-10-30', description: '主体申请通过' },
        { date: '2023-10-24', description: '首次提交主体申请' },
        { date: '2023-10-23', description: 'ICP备案通过' },
        { date: '2023-10-14', description: '第二个域名提交ICP备案' },
        { date: '2023-10-13', description: '购买第二个域名101jc.com' },
        { date: '2023-05-17', description: '首个域名ICP备案通过' },
        { date: '2023-05-10', description: '首次提交ICP备案' },
        { date: '2023-05-08', description: '购买第一个域名jcgjx.cn' },
        { date: '2023-05-08', description: '购买第一台云服务器' },
      ],
    };
  },
};
</script>

<style scoped>
.left-div {
  flex-shrink: 0;
  width: 100%;
  height: 100%;
  border-radius: 13px;
  margin-top: 15px;
  padding: 20px;
  backdrop-filter: 0px;

}
#line {
    width: 300px;
    height: 500px;
    font-size: 13px;
    padding-left: 100px;
    scroll-snap-type: y mandatory;
    overflow-y: scroll;
    /* 隐藏滚动条，但仍然允许滚动 */
    -ms-overflow-style: none;  /* Internet Explorer和Edge */
    scrollbar-width: none;  /* Firefox */
    overflow: -moz-scrollbars-none; /* 旧的Mozilla浏览器 */
}

#line::-webkit-scrollbar {
    display: none; /* 对于基于Webkit的浏览器，如Chrome、Safari等 */
}

#line li {
    list-style: none;
    position: relative;
    padding: 15px 0px 0px 15px;
    border-left: 2px solid #d5d5d5;
    border-radius: 0;
    scroll-snap-align: end;
    color: var(--main-text-color);
}
#line li:first-child .focus:first-child {
    background-color: #aaffcd;
    animation: colorFlash 1s ease infinite;
}
.first-event-date {
  color: #ffffff; /* 第一个事件的日期颜色 */
}
.event-date {
  color: #dddddd; /* 其他事件的日期颜色 */
}
.first-event-description {
  color: #ffffff; /* 第一个事件的描述颜色 */
}
.event-description {
  color: #dddddd; /* 其他事件的描述颜色 */
}

@keyframes colorFlash {
  0% { background-color: #aaffcd; }
  50% { background-color: #00FDA1; }
  100% { background-color: #aaffcd; }
}
.focus {
    width: 15px;
    height: 15px;
    border-radius: 22px;
    background-color: rgb(255 255 255);
    border: 2px solid #fff;
    position: absolute;
    left: -9px;
    top: 50%;
    
}

</style>
