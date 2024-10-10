<template>
  <div class="container mx-auto p-4 space-y-4">
    <el-row :gutter="20">
      <el-col :span="12">
        <el-card>
          <template #header>
            <div class="card-header">
              <span>Visitor Flowrate</span>
            </div>
          </template>
          <v-chart :option="flowrateChartOption" style="height: 300px;" />
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card>
          <template #header>
            <div class="card-header">
              <span>User Experience Distribution</span>
            </div>
          </template>
          <v-chart :option="expDistributionChartOption" style="height: 300px;" />
        </el-card>
      </el-col>
    </el-row>

    <el-card>
      <template #header>
        <div class="card-header">
          <span>Face Detection Records</span>
        </div>
      </template>
      <el-table :data="tableData" stripe>
        <el-table-column align="center" width="55"></el-table-column>
        <el-table-column align="center" label="ID" prop="id" sortable width="80"></el-table-column>
        <el-table-column label="username" prop="username" show-overflow-tooltip></el-table-column>
        <el-table-column label="nickname" prop="nickname" show-overflow-tooltip></el-table-column>
        <el-table-column label="email" prop="email" show-overflow-tooltip></el-table-column>
        <el-table-column label="exp" prop="exp" show-overflow-tooltip></el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { use } from 'echarts/core'
import { CanvasRenderer } from 'echarts/renderers'
import { LineChart, PieChart } from 'echarts/charts'
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent,
  GridComponent
} from 'echarts/components'
import VChart from 'vue-echarts'

use([
  CanvasRenderer,
  LineChart,
  PieChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent,
  GridComponent
])

const robotPosition = ref({ x: 0, y: 0 })

const moveRobot = (direction) => {
  const step = 10
  switch (direction) {
    case 'up':
      robotPosition.value.y -= step
      break
    case 'down':
      robotPosition.value.y += step
      break
    case 'left':
      robotPosition.value.x -= step
      break
    case 'right':
      robotPosition.value.x += step
      break
  }
}

// const mockDetectionRecords = [
//   { time: "2023-05-01 09:15", username: "John Doe", exp: 1500, email: "john@example.com" },
//   { time: "2023-05-01 09:20", username: "Jane Smith", exp: 2200, email: "jane@example.com" },
//   { time: "2023-05-01 09:25", username: "Bob Johnson", exp: 1800, email: "bob@example.com" },
// ]

const mockFlowrateData = [
  { time: "09:00", visitors: 5 },
  { time: "10:00", visitors: 12 },
  { time: "11:00", visitors: 18 },
  { time: "12:00", visitors: 25 },
  { time: "13:00", visitors: 20 },
  { time: "14:00", visitors: 22 },
]

const mockExpData = [
  { name: "Beginner", value: 30 },
  { name: "Intermediate", value: 45 },
  { name: "Expert", value: 25 },
]

const flowrateChartOption = {
  xAxis: {
    type: 'category',
    data: mockFlowrateData.map(item => item.time)
  },
  yAxis: {
    type: 'value'
  },
  series: [{
    data: mockFlowrateData.map(item => item.visitors),
    type: 'line'
  }]
}

const expDistributionChartOption = {
  series: [{
    type: 'pie',
    data: mockExpData,
    radius: '50%'
  }]
}
</script>

<script>
export default {
  name: "UserPunchInInfo",
  data() {
    return {
      tableData: [],  // 所有的数据
      fromVisible: false,
      form: {},
      isEdit: false,
      rules: {
        id: [
          {required: true, message: 'please enter content', trigger: 'blur'},
        ],
        username: [
          {required: true, message: 'please enter content', trigger: 'blur'},
        ],
        password: [
          {required: true, message: 'please enter content', trigger: 'blur'},
        ],
        nickname: [
          {required: true, message: 'please enter content', trigger: 'blur'},
        ],
        email: [
          {required: true, message: 'please enter content', trigger: 'blur'},
        ],
        exp: [
          {required: true, message: 'please enter content', trigger: 'blur'},
        ],
      },
    }
  },
  created() {
    this.loadTableData();
  },
  methods: {
    loadTableData(){
      this.$request.get('/user/').then(res => {
        this.tableData = res || []
      })
    },
  }
}
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.el-button {
  width: 100%;
}

.el-row{
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
}

.el-card{
  margin-top: 10px;
  border-radius: 10px;
}
</style>