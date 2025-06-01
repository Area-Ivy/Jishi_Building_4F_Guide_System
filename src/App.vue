<template>
  <div class="app">
    <div class="main-layout">
      <!-- 左侧区域 -->
      <div class="left-section">
        <!-- 标题栏 -->
        <div class="title-bar">
          <h1 class="title">济事楼教学楼导览 - 计算机科学与技术学院</h1>
        </div>
        <!-- 地图区域 -->
        <div class="map-section">
          <MapView 
            @room-selected="handleRoomSelect"
            ref="mapViewRef"
          />
        </div>
      </div>
      
      <!-- 右侧信息区域 -->
      <div class="info-section">
        <SearchBox @search="handleSearch" />
        <InfoPanel 
          :selected-room="selectedRoom"
          :search-results="searchResults"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import MapView from './components/MapView.vue'
import SearchBox from './components/SearchBox.vue'
import InfoPanel from './components/InfoPanel.vue'

const selectedRoom = ref(null)
const searchResults = ref([])
const mapViewRef = ref(null)

onMounted(() => {
  console.log('App mounted, mapViewRef:', mapViewRef.value);
  if (mapViewRef.value) {
    console.log('Initial rooms data:', mapViewRef.value.rooms);
  }
});

const handleRoomSelect = (room) => {
  const enhancedRoom = {
    ...room,
    teachers: room.teachers.map((name, index) => ({
      id: `${room.id}-teacher-${index}`,
      name,
      title: '教师'  
    }))
  }
  selectedRoom.value = enhancedRoom
  searchResults.value = []
}

const handleSearch = (query) => {
  console.log('Search query:', query);
  
  if (!query) {
    searchResults.value = []
    return
  }

  if (!mapViewRef.value) {
    console.error('MapView component not initialized');
    return;
  }

  const roomsData = mapViewRef.value.rooms;
  console.log('Rooms data:', roomsData);

  if (!roomsData || !Array.isArray(roomsData)) {
    console.error('Rooms data not available or invalid');
    return;
  }

  try {
    const results = []
    const searchText = query.toLowerCase().trim()

    roomsData.forEach(room => {
      const isRoomNumberMatch = room.name.toLowerCase().includes(searchText)
      const isRoomTypeMatch = room.type.toLowerCase().includes(searchText)
      const matchedTeachers = room.teachers.filter(name => 
        name.toLowerCase().includes(searchText)
      )

      if (isRoomNumberMatch || isRoomTypeMatch) {
        room.teachers.forEach((name, index) => {
          results.push({
            teacher: {
              id: `${room.id}-teacher-${index}`,
              name,
              title: '教师'
            },
            room: {
              id: room.id,
              name: room.name,
              type: room.type
            },
            matchType: isRoomNumberMatch ? 'room-number' : 'room-type'
          })
        })
      } else if (matchedTeachers.length > 0) {
        matchedTeachers.forEach((name, index) => {
          results.push({
            teacher: {
              id: `${room.id}-teacher-${index}`,
              name,
              title: '教师'
            },
            room: {
              id: room.id,
              name: room.name,
              type: room.type
            },
            matchType: 'teacher'
          })
        })
      }
    })

    console.log('Search results:', results);
    searchResults.value = results
    if (results.length > 0) {
      selectedRoom.value = null
    }
  } catch (error) {
    console.error('Error during search:', error);
    searchResults.value = []
  }
}
</script>

<style>
:root {
  --primary-color: #67C23A;
  --primary-light: #85ce61;
  --primary-dark: #529b2e;
  --background-color: #f0f9eb;
}

body {
  margin: 0;
  padding: 0;
  background: var(--background-color);
}

.app {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.main-layout {
  display: flex;
  width: 100%;
  height: 100%;
}

.left-section {
  flex: 1;
  height: 100%;
  display: flex;
  flex-direction: column;
  padding: 20px;
  padding-top: 8px;
}

.title-bar {
  background: linear-gradient(135deg, #66b1ff, #36CFC9);
  padding: 10px 20px;
  text-align: center;
  box-shadow: 0 2px 12px rgba(102, 177, 255, 0.15);
  border-radius: 8px;
  margin-bottom: 8px;
  position: relative;
  overflow: hidden;
  border: 1px solid rgba(102, 177, 255, 0.1);
}

.title-bar::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(to right, 
    rgba(255, 255, 255, 0.1),
    rgba(255, 255, 255, 0.2)
  );
  transform: skewX(-15deg) translateX(-50%);
  pointer-events: none;
}

.title {
  margin: 0;
  color: white;
  font-size: 16px;
  font-weight: 500;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  letter-spacing: 0.5px;
}

.map-section {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  background: white;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
  border: 1px solid rgba(103, 194, 58, 0.1);
}

.info-section {
  width: 400px;
  height: 100%;
  background: white;
  box-shadow: -4px 0 12px rgba(0, 0, 0, 0.05);
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 20px;
  border-left: 1px solid rgba(103, 194, 58, 0.1);
}
</style>