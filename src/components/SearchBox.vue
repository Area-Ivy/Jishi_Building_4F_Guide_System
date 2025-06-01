<template>
  <div class="search-box">
    <el-input
      v-model="searchQuery"
      placeholder="输入教师姓名或办公室门牌号搜索"
      clearable
      @input="handleSearch"
      @clear="handleClear"
      size="large"
      class="search-input"
    >
      <template #prefix>
        <el-icon><Search /></el-icon>
      </template>
    </el-input>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import { Search } from '@element-plus/icons-vue'

const searchQuery = ref('')
const emit = defineEmits(['search'])

const handleSearch = () => {
  try {
    emit('search', searchQuery.value)
  } catch (error) {
    console.error('Error in search:', error)
  }
}

const handleClear = () => {
  searchQuery.value = ''
  emit('search', '')
}

watch(searchQuery, (newVal) => {
  if (!newVal) {
    emit('search', '')
  }
})
</script>

<style scoped>
.search-box {
  width: 100%;
}

.search-input {
  width: 100%;
}

:deep(.el-input__wrapper) {
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

:deep(.el-input__wrapper:hover) {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
</style> 