<template>
  <div class="info-panel">
    <template v-if="selectedRoom">
      <h3 class="panel-title">{{ selectedRoom.name }} - {{ selectedRoom.type }}</h3>
      <div class="teacher-list">
        <template v-if="selectedRoom.teachers.length > 0">
          <div v-for="teacher in selectedRoom.teachers" :key="teacher.id" class="teacher-item">
            <div class="avatar">{{ teacher.name[0] }}</div>
            <div class="teacher-meta">
              <span class="teacher-name">{{ teacher.name }}</span>
              <el-tag size="small" type="info">{{ teacher.title }}</el-tag>
            </div>
          </div>
        </template>
        <template v-else>
          <div class="empty-teachers">
            <el-empty description="暂无人员信息" />
          </div>
        </template>
      </div>
    </template>
    <template v-else-if="searchResults.length > 0">
      <h3 class="panel-title">搜索结果</h3>
      <div class="search-results">
        <template v-if="roomResults.length > 0">
          <div class="result-group">
            <div class="group-title">
              <el-tag type="success" size="small">办公室</el-tag>
            </div>
            <div v-for="result in roomResults" :key="result.teacher.id" class="result-item">
              <div class="avatar">{{ result.teacher.name[0] }}</div>
              <div class="teacher-meta">
                <span class="teacher-name">{{ result.teacher.name }}</span>
                <el-tag size="small" type="info">{{ result.teacher.title }}</el-tag>
                <div class="room-info">
                  {{ result.room.name }} - {{ result.room.type }}
                </div>
              </div>
            </div>
          </div>
        </template>
        
        <template v-if="teacherResults.length > 0">
          <div class="result-group">
            <div class="group-title">
              <el-tag type="primary" size="small">教师</el-tag>
            </div>
            <div v-for="result in teacherResults" :key="result.teacher.id" class="result-item">
              <div class="avatar">{{ result.teacher.name[0] }}</div>
              <div class="teacher-meta">
                <span class="teacher-name">{{ result.teacher.name }}</span>
                <el-tag size="small" type="info">{{ result.teacher.title }}</el-tag>
                <div class="room-info">
                  {{ result.room.name }} - {{ result.room.type }}
                </div>
              </div>
            </div>
          </div>
        </template>
      </div>
    </template>
    <template v-else>
      <div class="empty-info">
        <el-empty description="点击地图上的办公室或搜索教师姓名、门牌号查看详细信息" />
      </div>
    </template>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  selectedRoom: {
    type: Object,
    default: null
  },
  searchResults: {
    type: Array,
    default: () => []
  }
})

const roomResults = computed(() => 
  props.searchResults.filter(result => 
    result.matchType === 'room-number' || result.matchType === 'room-type'
  )
)

const teacherResults = computed(() => 
  props.searchResults.filter(result => result.matchType === 'teacher')
)
</script>

<style scoped>
.info-panel {
  flex: 1;
  background: white;
  border-radius: 12px;
  padding: 20px;
  overflow-y: auto;
}

.panel-title {
  margin: 0 0 20px 0;
  color: #333;
  font-size: 18px;
  font-weight: 600;
}

.teacher-list, .search-results {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.result-group {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.group-title {
  margin: 8px 0;
}

.teacher-item, .result-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px;
  background: #f5f7fa;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.teacher-item:hover, .result-item:hover {
  background: #f0f2f5;
}

.avatar {
  width: 40px;
  height: 40px;
  background: linear-gradient(135deg, #409EFF 0%, #66b1ff 100%);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  font-weight: 500;
}

.teacher-meta {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.teacher-name {
  font-size: 16px;
  color: #333;
  font-weight: 500;
}

.room-info {
  font-size: 14px;
  color: #666;
}

.empty-info, .empty-teachers {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #999;
}

.empty-teachers {
  min-height: 200px;
}
</style>