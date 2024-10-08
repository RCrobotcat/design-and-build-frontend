<template>
  <div>
    <!--    <div class="search">-->
    <!--      <el-input placeholder="请输入标题查询" style="width: 200px" v-model="title"></el-input>-->
    <!--      <el-button type="info" plain style="margin-left: 10px" @click="load(1)">查询</el-button>-->
    <!--      <el-button type="warning" plain style="margin-left: 10px" @click="reset">重置</el-button>-->
    <!--    </div>-->

    <div class="operation">
      <el-button type="primary" plain @click="handleAdd">ADD</el-button>
    </div>

    <div class="table">
      <el-table :data="tableData" stripe>
        <el-table-column align="center" width="55"></el-table-column>
        <el-table-column align="center" label="ID" prop="id" sortable width="80"></el-table-column>
        <el-table-column label="username" prop="username" show-overflow-tooltip></el-table-column>
        <el-table-column label="password" prop="password" show-overflow-tooltip></el-table-column>
        <el-table-column label="nickname" prop="nickname" show-overflow-tooltip></el-table-column>
        <el-table-column label="email" prop="email" show-overflow-tooltip></el-table-column>
        <el-table-column label="exp" prop="exp" show-overflow-tooltip></el-table-column>
        <el-table-column label="face image" prop="face_image" show-overflow-tooltip></el-table-column>

        <el-table-column align="center" label="Actions" width="180">
          <template v-slot="scope">
            <el-button plain size="mini" type="danger" @click=del(scope.row.username)>Delete</el-button>
          </template>
        </el-table-column>
      </el-table>

      <el-dialog :close-on-click-modal="false" :visible.sync="fromVisible" destroy-on-close title="Info" width="40%">
        <el-form ref="formRef" :model="form" :rules="rules" label-width="100px" style="padding-right: 50px">
          <el-form-item label="ID" prop="id">
            <el-input v-model="form.id" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="username" prop="username">
            <el-input v-model="form.username" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="nickname" prop="nickname">
            <el-input v-model="form.nickname" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="email" prop="email">
            <el-input v-model="form.email" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="password" prop="password">
            <el-input v-model="form.password" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="exp" prop="exp">
            <el-input v-model="form.exp" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="Face Image" prop="face_image">
            <el-upload
                class="avatar-uploader"
                :action="$baseUrl + '/image/'"
                :show-file-list="false"
                :on-success="handleAvatarSuccess"
            >
              <img v-if="form.face_image" :src="form.face_image" class="avatar" />
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            </el-upload>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="fromVisible = false">Cancel</el-button>
          <el-button type="primary" @click="save">Save</el-button>
        </div>
      </el-dialog>

    </div>
  </div>
</template>

<script>
export default {
  name: "Admin",
  data() {
    return {
      tableData: [],  // 所有的数据
      fromVisible: false,
      form: {},
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
        face_image: [
          {required: true, message: 'please upload image', trigger: 'blur'},
        ],
      },
    }
  },
  created() {
    this.loadTableData();
  },
  methods: {
    handleEdit(row) {   // 编辑数据
      this.form = JSON.parse(JSON.stringify(row))  // 给form对象赋值  注意要深拷贝数据
      this.fromVisible = true   // 打开弹窗
    },
    loadTableData(){
      this.$request.get('/user/').then(res => {
        this.tableData = res || []
      })
    },
    handleAdd() {   // 新增数据
      this.form = {}  // 新增数据的时候清空数据
      this.fromVisible = true   // 打开弹窗
    },
    save() {
      this.$refs.formRef.validate((valid) => {
        if (valid) {
          this.$request.post('/user/', this.form).then(res => {
            if (res.message === 'User created successfully.') {
              this.$message.success('User created successfully.');
              this.loadTableData();
              this.fromVisible = false;
            } else if (res.detail && res.detail.length > 0) {
              this.$message.error(res.detail[0].msg);
            }
          }).catch(error => {
            this.$message.error('An error occurred while creating the user.');
          });
        }
      });
    },
    del(username){
      this.$confirm('Are you sure you want to delete it?', 'Confirm deletion', {type: "warning"}).then(response => {
        this.$request.delete('/user/' + username).then(res => {
          if (!res) {
            this.$message.success('User deleted successfully.');
            this.loadTableData();
          } else {
            this.$message.error('An error occurred while deleting the user.');
          }
        }).catch(error => {
          this.$message.error('An error occurred while deleting the user.');
        });
      }).catch(() => {
      });
    },
    handleAvatarSuccess(response, file, fileList) {
      this.$set(this.tableData, 'face_image', response)
    },
  }
}
</script>

<style scoped>

</style>
