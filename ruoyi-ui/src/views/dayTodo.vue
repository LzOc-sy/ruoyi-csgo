<template>
  <div class="common-layout">
    <el-container>
      <el-header class="minimalist-header">
        <div class="header-content">
          <i class="el-icon-bell">
          <span class="title">失败乃成功之母</span></i>
          <div class="bottom-left">
            <el-button type="primary" class="white-btn" icon="el-icon-notebook-2" color="#ffffff">列表视图</el-button>
            <el-button type="primary" class="white-btn" icon="el-icon-date">日历视图</el-button>
            <el-button type="primary" class="white-btn" icon="el-icon-edit">添加任务</el-button>
          </div>
        </div>
      </el-header>
      <el-main>
        <div >

          <TimeDisplay
            :show-seconds="true"
            :show-date="true"
            :show-day="true"
            :use12-hour="false"
            size="medium"
            layout="standard"
            color="#409EFF"
          /></i>
        </div>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import TimeDisplay from '@/components/display/TimeDisplay.vue'
export default {
  name: "DayTodo",
  components: {
    TimeDisplay
  },
  data() {
    return {
      version: "3.6.3"
    }
  },
};
</script>

<style scoped lang="scss">
.common-layout {
  height: 100vh;

  .minimalist-header {
    height: 80px;
    background: #ffffff;
    border-bottom: 1px solid #e4e7ed; /* 建议只保留底部边框 */
    padding: 0 24px;
    display: flex;
    align-items: center;
  }

  .header-content {
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .white-btn{
    background-color: #ffffff;
    border-color: #dcdfe6;
    color: #606266;

    &:hover,
    &:focus {
      background-color: #f5f7fa;
      border-color: #c6e2ff;

    }

    &:active {
      background-color: #ecf5ff;
      border-color: #409EFF;
    }
  }

  .el-icon-bell {
    display: flex;
    align-items: center;
    gap: 16px;
  }

  .title {
    display: flex;
    font-size: 18px;
    font-weight: 600;
    color: #303133;
  }

  /* 其他原有样式 */
  blockquote {
    display: flex;
    padding: 10px 20px;
    margin: 0 0 20px;
    font-size: 17.5px;
    border-left: 5px solid #eee;
  }
}
</style>
