<template>
  <div class="map-container" ref="mapContainer">
    <img :src="mapImageSrc" alt="教学楼地图" class="map-image" @load="handleImageLoad" ref="mapImage"/>
    <div 
      v-for="room in scaledRooms" 
      :key="room.id"
      class="room-area"
      :style="{
        left: `${room.scaledX}%`,
        top: `${room.scaledY}%`
      }"
      @click="handleRoomClick(room)"
    >
      <el-tooltip
        :content="`${room.name} - ${room.type}`"
        placement="top"
        effect="light"
      >
        <div class="room-dot" :class="getRoomTypeClass(room.type)"></div>
      </el-tooltip>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import mapImageSrc from '../assets/济事楼地图.jpg'

const ORIGINAL_WIDTH = 1200; 
const ORIGINAL_HEIGHT = 800; 

const mapContainer = ref(null);
const mapImage = ref(null);
const scale = ref(1);

const scaledRooms = computed(() => {
  return rooms.value.map(room => ({
    ...room,
    scaledX: (room.x / ORIGINAL_WIDTH) * 100,
    scaledY: (room.y / ORIGINAL_HEIGHT) * 100
  }));
});

const handleImageLoad = () => {
  updateScale();
};

const updateScale = () => {
  if (mapImage.value) {
    const rect = mapImage.value.getBoundingClientRect();
    scale.value = rect.width / ORIGINAL_WIDTH;
  }
};

const handleResize = () => {
  updateScale();
};

onMounted(() => {
  window.addEventListener('resize', handleResize);
  updateScale();
  console.log('MapView mounted, rooms data:', rooms.value);
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
});

const parseOccupants = (occupantStr) => {
  if (occupantStr === '暂无') return [];
  return occupantStr.split(' ').filter(name => name.length > 0);
}

const rooms = ref([
{
    id: 'room407',
    name: '407',
    type: '研究生工作室',
    x: 565,
    y: 515,
    teachers: parseOccupants('陈梁 严海洲 叶珂男')
  },
  {
    id: 'room408',
    name: '408',
    type: 'MultiMedia Lecture Room',
    x: 510,
    y: 595,
    teachers: parseOccupants('暂无')
  },
  {
    id: 'room409',
    name: '409',
    type: '(左)研究生工作室 (右)研究生工作室',
    x: 520,
    y: 475,
    teachers: parseOccupants('陈梁 朱亚萍 代玉琢 杜庆峰')
  },
  {
    id: 'room410',
    name: '410',
    type: '同济大学苹果开发技术试验室',
    x: 475,
    y: 565,
    teachers: parseOccupants('王冬青 李江峰 夏波涌 张颖')
  },
  {
    id: 'room412',
    name: '412',
    type: 'Cisco网络与信息安全实验室',
    x: 450,
    y: 535,
    teachers: parseOccupants('刘岩 张惠娟 孙萍 罗怡桂')
  },
  {
    id: 'room413',
    name: '413',
    type: '弱电间',
    x: 430,
    y: 380,
    teachers: parseOccupants('暂无')
  },
  {
    id: 'room414',
    name: '414',
    type: '硬件实验室',
    x: 355,
    y: 385,
    teachers: parseOccupants('暂无')
  },
  {
    id: 'room415',
    name: '415',
    type: '强电间',
    x: 450,
    y: 380,
    teachers: parseOccupants('暂无')
  },
  {
    id: 'room416',
    name: '416',
    type: '计算机房3',
    x: 355,
    y: 300,
    teachers: parseOccupants('杨旻 严海洲')
  },
  {
    id: 'room417',
    name: '417',
    type: '会议室',
    x: 450,
    y: 300,
    teachers: parseOccupants('暂无')
  },
  {
    id: 'room418',
    name: '418',
    type: '(左)教授办公室 (右)研究生工作室',
    x: 355,
    y: 210,
    teachers: parseOccupants('张林 江建慧 刘琴 袁时金 陈梁 尹长青 陈韩悦')
  },
  {
    id: 'room419',
    name: '419',
    type: '嵌入式系统实验室',
    x: 450,
    y: 210,
    teachers: parseOccupants('张晶 严海洲')
  },
  {
    id: 'room426',
    name: '426',
    type: '多媒体教学机房',
    x: 430,
    y: 50,
    teachers: parseOccupants('陈梁 杨旻')
  },
  {
    id: 'room428',
    name: '428',
    type: '服务器机房',
    x: 470,
    y: 50,
    teachers: parseOccupants('杨旻 严海洲')
  },
  {
    id: 'room430',
    name: '430',
    type: '多媒体教学机房',
    x: 510,
    y: 65,
    teachers: parseOccupants('陈梁 杨旻')
  },
  {
    id: 'room432',
    name: '432',
    type: '党员之家',
    x: 623,
    y: 58,
    teachers: parseOccupants('暂无')
  },
  {
    id: 'room434',
    name: '434',
    type: '多媒体教室',
    x: 645,
    y: 58,
    teachers: parseOccupants('杨旻')
  },
  {
    id: 'room441',
    name: '441',
    type: '会议室',
    x: 710,
    y: 210,
    teachers: parseOccupants('暂无')
  },
  {
    id: 'room442',
    name: '442',
    type: '(左)学院办公室 (右)教务办公室',
    x: 635,
    y: 170,
    teachers: parseOccupants('刘梦露 李慧敏 王彩霞 杨丹 姚仕仪 闫鹏 张晶 林伊凡 张晓雅 钱银飞 王昊榕 俞晓静')
  },
  {
    id: 'room443',
    name: '443',
    type: '实验中心',
    x: 690,
    y: 270,
    teachers: parseOccupants('杨旻 陈梁 严海洲')
  },
  {
    id: 'room444',
    name: '444',
    type: '档案室',
    x: 623,
    y: 240,
    teachers: parseOccupants('暂无')
  },
  {
    id: 'room446',
    name: '446',
    type: '学生工作办公室',
    x: 623,
    y: 272,
    teachers: parseOccupants('张砚秋 丁瑞庭 葛蕾 焦嘉欣 钟梦莹 陈璞皎')
  },
  {
    id: 'room448',
    name: '448',
    type: '(1)副书记办公室 (2)副书记办公室 (3)院务助理办公室',
    x: 628,
    y: 318,
    teachers: parseOccupants('陈荣 吴晓培 宋井宽')
  },
  {
    id: 'room450',
    name: '450',
    type: '(左)党委书记办公室 (右)院长办公室',
    x: 650,
    y: 350,
    teachers: parseOccupants('熊岚 申恒涛')
  },
  {
    id: 'room451',
    name: '451',
    type: '副院长办公室',
    x: 730,
    y: 315,
    teachers: parseOccupants('王成 何良华 张林')
  },
  {
    id: 'room455',
    name: '455',
    type: '会议室',
    x: 810,
    y: 390,
    teachers: parseOccupants('暂无')
  },
  {
    id: 'room456',
    name: '456',
    type: '党委办公室',
    x: 770,
    y: 450,
    teachers: parseOccupants('周徽徽 陆凤岚 赵清理')
  }
])

const emit = defineEmits(['room-selected'])

const handleRoomClick = (room) => {
  emit('room-selected', room)
}

const getRoomTypeClass = (type) => {
  const typeMap = {
    '教研室': 'dot-research',
    '研究生工作室': 'dot-graduate',
    '多媒体教室': 'dot-multimedia',
    '实验室': 'dot-lab',
    '会议室': 'dot-meeting',
    '办公室': 'dot-office',
    '院长办公室': 'dot-dean',
    '学院办公室': 'dot-admin',
    '党员之家': 'dot-party'
  }
  
  for (const [key, value] of Object.entries(typeMap)) {
    if (type.includes(key)) return value;
  }
  return 'dot-default';
}

const roomsData = computed(() => rooms.value)

defineExpose({
  rooms: roomsData,
  getRoomById: (id) => rooms.value.find(room => room.id === id)
})
</script>

<style scoped>
.map-container {
  position: relative;
  display: inline-block;
  background: white;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
  width: 100%;
}

.map-image {
  width: 100%;
  height: auto;
  border-radius: 8px;
}

.room-area {
  position: absolute;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transform: translate(-50%, -50%);
}

.room-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  transition: all 0.3s ease;
  box-shadow: 0 2px 6px rgba(102, 177, 255, 0.2);
  border: 2px solid #66b1ff;
  background: #a4d0ff;
}

.room-area:hover .room-dot {
  transform: scale(1.3);
  box-shadow: 0 2px 8px rgba(102, 177, 255, 0.3);
}

.dot-research, .dot-graduate, .dot-multimedia, .dot-lab, 
.dot-meeting, .dot-office, .dot-dean, .dot-admin, 
.dot-party, .dot-default { 
  background: #a4d0ff;
  border: 2px solid #66b1ff;
}
</style>