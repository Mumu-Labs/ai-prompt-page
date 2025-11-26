<template>
  <el-card class="home-card">
    <p class="intro-text">这里是您的AI提示词库，汇集了多种场景下的提示词模板。</p>
    <p class="intro-text">您可以复制这些提示词到各种大模型中，如豆包 、Kimi等，快速生成您所需的图片。</p>

    <el-row :gutter="20" style="margin-top: 30px;">
      <!-- 文生图提示词 -->
      <el-col class="home-col" :span="24">
        <el-card @click.native="routerJump('text-to-image')" shadow="hover" class="feature-card">
          <span>📋文生图提示词</span>
        </el-card>
      </el-col>
      <!-- P图提示词 -->
      <el-col class="home-col" :span="24">
        <el-card @click.native="routerJump('image-to-image')" shadow="hover" class="feature-card">
          <span>🖼️ P图提示词</span>
        </el-card>
      </el-col>
    </el-row>
  </el-card>
</template>

<script>
export default {
  name: 'HomePage',
  mounted() {
    // 可以在这里添加页面加载后的逻辑
  },
  methods: {
    routerJump(path) {
      // 简化判断逻辑
      if (this.$route.path !== `/${path}`) {
        this.$router.push(`/${path}`);
      }
    }
  }
}
</script>

<style scoped>
/* --- 主卡片样式 --- */
.home-card {
  max-width: 100%;
  margin: 20px auto; /* 上下间距，居中 */
  text-align: left;
  border-radius: 12px; /* 更大的圆角 */
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08); /* 更柔和的阴影 */
  border: 1px solid #e0e0e0; /* 细边框 */
  padding: 25px; /* 增加内边距 */
  /* 可选：添加背景渐变 */
  /* background: linear-gradient(to bottom, #ffffff, #f9f9f9); */
  /* 可选：添加边框 */
  /* border: 1px solid rgba(0, 0, 0, 0.05); */
}

/* --- 欢迎标题样式 --- */
.welcome-title {
  margin: 0;
  color: #2c3e50; /* 深色文字 */
  font-size: 24px; /* 增大字体 */
  font-weight: 600; /* 加粗 */
  text-align: center; /* 居中对齐 */
  line-height: 1.4; /* 行高 */
  /* 可选：添加一点渐变或阴影效果 */
  /* background: linear-gradient(to right, #409EFF, #66b1ff); */
  /* -webkit-background-clip: text; */
  /* -webkit-text-fill-color: transparent; */
}

/* --- 介绍文字样式 --- */
.intro-text {
  margin: 15px 0; /* 上下间距 */
  color: #5a67d8; /* 深蓝色 */
  font-size: 16px; /* 字体大小 */
  line-height: 1.6; /* 行高，提高可读性 */
  text-align: justify; /* 两端对齐 */
  /* 可选：添加一点颜色 */
  /* color: #333; */
  /* 可选：添加一点内边距 */
  /* padding: 0 10px; */
}

/* --- 卡片容器 --- */
.home-col {
  margin-bottom: 20px; /* 卡片间间距 */
}

/* --- 特色卡片样式 --- */
.feature-card {
  cursor: pointer;
  border-radius: 10px; /* 圆角 */
  border: 1px solid #e0e0e0; /* 边框 */
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1); /* 平滑过渡 */
  /* 可选：添加背景渐变 */
  background: linear-gradient(to bottom, #ffffff, #f8f8f8);
}

.feature-card:hover {
  transform: translateY(-5px); /* 悬停时轻微上移 */
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.12); /* 更强的阴影 */
  border-color: #4CAF50; /* 悬停时边框颜色 */
  /* 可选：添加一点边框阴影 */
  /* box-shadow: 0 12px 30px rgba(0, 0, 0, 0.12), 0 0 0 2px #4CAF50; */
}

/* --- 卡片头部样式 --- */
.card-header {
  display: flex;
  align-items: center; /* 垂直居中 */
  padding: 15px 20px; /* 内边距 */
  background-color: #f8f9fa; /* 浅灰色背景 */
  border-radius: 10px 10px 0 0; /* 仅顶部圆角 */
  /* 可选：添加背景渐变 */
  /* background: linear-gradient(to right, #f8f9fa, #ffffff); */
}

/* --- 卡片图标样式 --- */
.icon-large {
  font-size: 22px; /* 增大图标 */
  color: #4CAF50; /* 图标颜色 */
  margin-right: 10px; /* 图标与文字间距 */
  /* 可选：添加一点阴影 */
  /* text-shadow: 0 1px 1px rgba(0, 0, 0, 0.05); */
}

/* --- 卡片标题样式 --- */
.card-title {
  font-size: 18px; /* 字体大小 */
  font-weight: 600; /* 加粗 */
  color: #2c3e50; /* 文字颜色 */
  /* 可选：添加一点颜色 */
  /* color: #4CAF50; */
}

/* --- 卡片内容样式 --- */
.card-content {
  padding: 20px; /* 内边距 */
}

.card-content p {
  margin: 10px 0; /* 段落间距 */
  color: #333; /* 文字颜色 */
  line-height: 1.6; /* 行高 */
  font-size: 15px; /* 字体大小 */
  /* 可选：添加一点颜色 */
  /* color: #555; */
}

/* --- 清除浮动 --- */
.clearfix::before,
.clearfix::after {
  display: table;
  content: "";
}

.clearfix::after {
  clear: both;
}

/* --- 响应式设计 --- */
@media (max-width: 768px) {
  .home-card {
    margin: 15px auto; /* 减少移动端间距 */
    padding: 10px; /* 减少内边距 */
  }

  .welcome-title {
    font-size: 22px; /* 移动端稍小 */
  }

  .intro-text {
    font-size: 15px; /* 移动端稍小 */
    margin: 12px 0; /* 减少间距 */
  }

  .card-header {
    padding: 12px 15px; /* 移动端内边距 */
  }

  .icon-large {
    font-size: 20px; /* 移动端图标稍小 */
    margin-right: 8px; /* 减少间距 */
  }

  .card-title {
    font-size: 16px; /* 移动端标题稍小 */
  }

  .card-content {
    padding: 15px; /* 移动端内边距 */
  }

  .card-content p {
    font-size: 14px; /* 移动端段落稍小 */
    margin: 8px 0; /* 减少段落间距 */
  }

  .home-col {
    margin-bottom: 15px; /* 减少卡片间距 */
  }
}
</style>
