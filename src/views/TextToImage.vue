<template>
  <el-card style="max-width: 100%; margin-bottom: 20px;">
    <div slot="header">
      <h3>📋 文生图 提示词</h3>
      <!-- 搜索框 -->
      <div class="search-container">
        <el-input
            v-model="searchKeyword"
            placeholder="搜索提示词标题..."
            clearable
            class="search-input"
            @input="handleSearch"
        >
          <i slot="prefix" class="el-input__icon el-icon-search"></i>
        </el-input>
      </div>

      <!-- 分类标签选择 -->
      <div class="category-tags-container">
        <el-tag
            v-for="category in categories"
            :key="category.value"
            :type="selectedCategory === category.value ? 'primary' : ''"
            :effect="selectedCategory === category.value ? 'dark' : 'light'"
            class="category-tag"
            @click="selectCategory(category.value)"
            :class="{ 'is-selected': selectedCategory === category.value }"
        >
          {{ category.label }}
        </el-tag>
      </div>
    </div>
    <el-row :gutter="20">
      <el-col
          v-for="item in filteredPromptsData"
          :key="item.id"
          :span="24"
          style="margin-bottom: 20px;">
        <el-card shadow="hover" class="prompt-card" @click.native="showDetailDialog(item)">
          <div slot="header" class="card-header">
            <span class="category-tag-small">{{ item.category }}</span>
            <!-- 动态显示分类 -->
            <span class="card-title">{{ item.title }}</span>
          </div>
          <!-- 使用计算属性控制显示长度 -->
          <p>{{ truncatedPrompt(item.prompt) }}</p>
        </el-card>
      </el-col>
    </el-row>

    <!-- 详情弹窗 -->
    <el-dialog
        :title="detailItem.title"
        :visible.sync="dialogVisible"
        width="80%"
        :before-close="handleClose"
        class="detail-dialog"
    >
      <div class="detail-content">
        <div class="prompt-section">
          <p class="prompt-text">{{ detailItem.prompt }}</p>
        </div>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button type="success" size="small" @click="copyFullPrompt" icon="el-icon-document-copy" :loading="loading">
          复制
        </el-button>
      </span>
    </el-dialog>
  </el-card>
</template>

<script>
// 导入新的嵌套结构数据
import { textToImagePrompts } from '@/assets/data/textToImagePrompts.js';

export default {
  name: 'TextToImagePage',
  data() {
    return {
      // 保存原始嵌套结构数据
      promptsData: textToImagePrompts,
      // 用于存储当前选择分类下的提示词数组 (过滤后，包含搜索结果)
      filteredPromptsData: [],
      // 用于存储分类选项
      categories: [],
      // 当前选中的分类
      selectedCategory: '',
      // 弹窗可见性
      dialogVisible: false,
      // 当前详情项
      detailItem: {},
      // 复制按钮加载状态
      loading: false,
      // 搜索关键词
      searchKeyword: '',
      // 原始数据（未过滤的），用于搜索
      originalPromptsData: [], // 新增：存储原始数据以便搜索
    };
  },
  methods: {
    // 截断提示词文本
    truncatedPrompt(text) {
      if (!text || text.length <= 20) {
        return text;
      }
      return text.substring(0, 30) + '...';
    },

    // 根据选中的分类过滤数据
    filterPrompts(value) {
      // 获取所有原始数据
      let allPrompts = [];
      Object.values(this.promptsData).forEach(categoryArray => {
        allPrompts = allPrompts.concat(
            categoryArray.map(item => ({
              ...item,
              category: Object.keys(this.promptsData)[Object.values(this.promptsData).indexOf(categoryArray)]
            }))
        );
      });

      // 保存原始数据
      this.originalPromptsData = allPrompts;

      // 根据分类筛选
      let filteredByCategory = [];
      if (value === '') {
        // 如果选择“全部”，则使用所有数据
        filteredByCategory = allPrompts;
      } else {
        // 获取指定分类下的数据
        const categoryArray = this.promptsData[value] || [];
        filteredByCategory = categoryArray.map(item => ({
          ...item,
          category: value
        }));
      }

      // 应用搜索过滤
      this.applySearchFilter(filteredByCategory);
    },

    /**
     * 应用搜索过滤器
     * @param {Array} baseData - 基础数据（已按分类筛选）
     */
    applySearchFilter(baseData) {
      if (!this.searchKeyword.trim()) {
        // 如果搜索关键词为空，则显示所有数据
        this.filteredPromptsData = baseData;
      } else {
        // 模糊搜索标题
        const keyword = this.searchKeyword.toLowerCase().trim();
        this.filteredPromptsData = baseData.filter(item =>
            item.title.toLowerCase().includes(keyword)
        );
      }
    },

    /**
     * 处理搜索事件
     */
    handleSearch() {
      // 确保在分类筛选后再应用搜索
      this.filterPrompts(this.selectedCategory);
    },

    /**
     * 选择分类
     * @param {string} categoryValue - 选中的分类值
     */
    selectCategory(categoryValue) {
      this.selectedCategory = categoryValue;
      this.filterPrompts(categoryValue); // 重新筛选
    },

    // 显示详情弹窗
    showDetailDialog(item) {
      this.detailItem = item; // 存储当前项用于弹窗显示
      this.dialogVisible = true; // 显示弹窗
    },

    // 关闭弹窗
    handleClose(done) {
      this.dialogVisible = false;
      done();
    },

    // 复制完整提示词
    async copyFullPrompt() {
      // 如果正在加载，则直接返回，不执行复制
      if (this.loading) {
        return;
      }
      this.loading = true; // 开始加载

      try {
        const fullPrompt = `正面提示词:\n${this.detailItem.prompt}\n\n负面提示词:\n${this.detailItem.negativePrompt}`;
        let result = "请给我生成一副这样的图片:\n" + fullPrompt;
        await navigator.clipboard.writeText(result);
        this.$message.success("已复制到剪贴板");
      } catch (err) {
        console.error('复制失败:', err);
        this.$message.error("复制失败，请手动复制。");

      } finally {
        // 无论成功还是失败，都设置 loading 为 false
        // 延迟 0.5 秒再设置，模拟复制操作的耗时
        setTimeout(() => {
          this.loading = false;
        }, 500);
      }
    }
  },
  mounted() {
    // 1. 生成分类选项列表
    // 从嵌套结构的键（分类名称）创建选项
    this.categories = [
      { label: '全部', value: '' },
      ...Object.keys(this.promptsData).map(key => ({ label: key, value: key }))
    ];

    // 2. 初始化显示所有数据
    this.selectCategory(''); // 调用 selectCategory 以初始化显示所有数据
  }
};
</script>

<!-- 引入共享样式 -->
<style scoped lang="css" src="@/styles/shared-prompts.css"></style>
