<template>
  <div class="container mx-auto p-4 space-y-4">
    <el-row :gutter="20">
      <el-col :span="12">
        <el-card>
          <template #header>
            <div class="card-header">
              <span style="font-weight: bold;">Face Detection Result</span>
            </div>
          </template>
          <div class="relative" style="display: flex;">
            <el-empty v-if="!detection_res || !detection_res.face_image" description="No image available" style="margin: auto;"></el-empty>
            <template v-else>
              <img
                  :src="detection_res.face_image"
                  alt="Face Detection"
                  class="w-full h-auto object-cover"
                  style="width: 80%; height: 80%; object-fit: cover;"
              />
              <span style="margin-left: 10px;">username:</span>
              <el-tag type="success" class="absolute top-2 right-2" style="margin-left: 10px; font-weight: bold;">
                {{ detection_res.username }}
              </el-tag>
            </template>
          </div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card style="height: 343px;">
          <template #header>
            <div class="card-header">
              <span style="font-weight: bold;">Robot Control Panel</span>
            </div>
          </template>
          <el-row :gutter="10" class="mb-4" style="margin-top: 30px;">
            <el-col :span="8"></el-col>
            <el-col :span="8">
              <el-button @click="moveRobot('forward')" icon="el-icon-arrow-up" type="info" plain>Forward</el-button>
            </el-col>
            <el-col :span="8"></el-col>
          </el-row>
          <el-row :gutter="10" class="mb-4">
            <el-col :span="8">
              <el-button @click="moveRobot('left')" icon="el-icon-arrow-left" type="info" plain>Left</el-button>
            </el-col>
            <el-col :span="8">
              <el-button @click="moveRobot('stop')" icon="el-icon-circle-close" type="warning" plain>Stop</el-button>
            </el-col>
            <el-col :span="8">
              <el-button @click="moveRobot('right')" icon="el-icon-arrow-right" type="info" plain>Right</el-button>
            </el-col>
          </el-row>
          <!--          <el-row :gutter="10" class="mb-4">-->
          <!--            <el-col :span="8"></el-col>-->
          <!--            <el-col :span="8">-->
          <!--              <el-button @click="moveRobot('backward')" icon="el-icon-arrow-down" type="info" plain>Backward</el-button>-->
          <!--            </el-col>-->
          <!--            <el-col :span="8"></el-col>-->
          <!--          </el-row>-->
          <el-row :gutter="10" class="mb-4">
            <el-col :span="5">
              <el-button @click="moveRobot('p')" icon="el-icon-camera" type="warning" plain>Capture</el-button>
            </el-col>
            <el-col :span="5">
              <el-button @click="moveRobot('ledon')" icon="el-icon-switch-button" type="danger" plain>LED On</el-button>
            </el-col>
            <el-col :span="5">
              <el-button @click="moveRobot('ledoff')" icon="el-icon-lightning" type="primary" plain>LED Off</el-button>
            </el-col>
          </el-row>
<!--          <div style="margin-top: 30px; font-size: 15px; text-align: center;">-->
<!--            Robot Position(Approximate): X: {{ robotPosition.x }}, Y: {{ robotPosition.y }}-->
<!--          </div>-->
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
import {use} from 'echarts/core'
import {CanvasRenderer} from 'echarts/renderers'
import {LineChart, PieChart} from 'echarts/charts'
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

// const robotPosition = ref({x: 0, y: 0})

// const moveRobot = (direction) => {
//   const step = 10
//   switch (direction) {
//       // case 'backward':
//       //   robotPosition.value.y -= step
//       //   break
//     case 'forward':
//       robotPosition.value.y += step
//       break
//     case 'left':
//       robotPosition.value.x -= step
//       break
//     case 'right':
//       robotPosition.value.x += step
//       break
//     case 'stop':
//       robotPosition.value.x = 0
//       robotPosition.value.y = 0
//       break
//   }
// }
</script>

<script>
export default {
  name: "UserPunchInInfo",
  data() {
    return {
      tableData: [],  // 所有的数据
      detection_res: {
        face_image: '',
        username: ''
      },
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
    // this.loadFaceDetection('stt');
    this.loadFaceDetection();
  },
  methods: {
    loadTableData() {
      this.$request.get('/user/').then(res => {
        this.tableData = res || []
      })
    },
    loadFaceDetection(){
      this.$request.get('/detection_result/').then(res => {
        if(res.annotated_image){
          this.detection_res.face_image = `data:image/jpeg;base64,${res.annotated_image}`;
          this.detection_res.username = res.username;
        }
      }).catch(err => {
        console.log(err)
      })
    },
    // loadFaceDetection(username) {
    //   this.$request.get(`/user/${username}`).then(res => {
    //     this.detection_res.id = res.id;
    //     this.detection_res.username = res.username;
    //     this.detection_res.email = res.email;
    //     this.detection_res.nickname = res.nickname;
    //     this.detection_res.face_image = `data:image/jpeg;base64,${res.face_image}`;
    //   })
    // },
    moveRobot(direction) {
      const step = 10
      switch (direction) {
        case 'forward':
          this.$request.post('/control/', { command: '1' }).then(res => {
            if (res.message === 'Command sent to robot.') {
              this.$message.success(res.message + ' Robot moving forward.')
            } else {
              this.$message.error('Command not sent to robot.')
            }
          })
          this.robotPosition.y += step
          break
        case 'left':
          this.$request.post('/control/', { command: '3' }).then(res => {
            if (res.message === 'Command sent to robot.') {
              this.$message.success(res.message + ' Robot moving left.')
            } else {
              this.$message.error('Command not sent to robot.')
            }
          })
          this.robotPosition.x -= step
          break
        case 'right':
          this.$request.post('/control/', { command: '4' }).then(res => {
            if (res.message === 'Command sent to robot.') {
              this.$message.success(res.message + ' Robot moving right.')
            } else {
              this.$message.error('Command not sent to robot.')
            }
          })
          this.robotPosition.x += step
          break
        case 'stop':
          this.$request.post('/control/', { command: '2' }).then(res => {
            if (res.message === 'Command sent to robot.') {
              this.$message.success(res.message + ' Robot stopped.')
            } else {
              this.$message.error('Command not sent to robot.')
            }
          })
          this.robotPosition.x = 0
          this.robotPosition.y = 0
          break
        case 'ledon':
          this.$request.post('/control/', { command: 'ledon' }).then(res => {
            if (res.message === 'Command sent to robot.') {
              this.$message.success(res.message + ' LED On.')
            } else {
              this.$message.error('Command not sent to robot.')
            }
          })
          break
        case 'ledoff':
          this.$request.post('/control/', { command: 'ledoff' }).then(res => {
            if (res.message === 'Command sent to robot.') {
              this.$message.success(res.message + ' LED Off.')
            } else {
              this.$message.error('Command not sent to robot.')
            }
          })
          break
        case 'p':
          this.$request.post('/control/', { command: 'p' }).then(res => {
            if (res.message === 'Command sent to robot.') {
              this.$message.success(res.message + ' Captured.')
            } else {
              this.$message.error('Command not sent to robot.')
            }
          })
          break
      }
    }
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

.el-row {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
}

.el-card {
  margin-top: 10px;
  border-radius: 10px;
}
</style>