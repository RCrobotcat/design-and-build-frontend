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

        <el-table-column align="center" label="Actions" width="180">
          <template v-slot="scope">
            <el-button plain size="mini" type="primary" @click=handleEdit(scope.row)>Edit</el-button>
            <el-button plain size="mini" type="danger" @click=del(scope.row.username)>Delete</el-button>
          </template>
        </el-table-column>
      </el-table>

      <el-dialog :close-on-click-modal="false" :visible.sync="fromVisible" destroy-on-close title="Info" width="40%">
        <el-form ref="formRef" :model="form" :rules="rules" label-width="100px" style="padding-right: 50px">
          <el-form-item v-if="!isEdit" label="username" prop="username">
            <el-input v-model="form.username" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="password" prop="password">
            <el-input v-model="form.password" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="nickname" prop="nickname">
            <el-input v-model="form.nickname" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item label="email" prop="email">
            <el-input v-model="form.email" autocomplete="off"></el-input>
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
                :before-upload="beforeUpload"
            >
              <img v-if="form.face_image" :src="form.face_image" style="width: 50px; height: 50px;" class="avatar" />
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            </el-upload>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="fromVisible = false; isEdit = false;">Cancel</el-button>
          <el-button type="primary" @click="save">Save</el-button>
        </div>
      </el-dialog>

    </div>
  </div>
</template>

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
    handleEdit(row) {   // 编辑数据
      this.isEdit = true;
      this.form = JSON.parse(JSON.stringify(row))  // 给form对象赋值  注意要深拷贝数据
      this.fromVisible = true   // 打开弹窗
    },
    loadTableData(){
      this.$request.get('/user/').then(res => {
        this.tableData = res || []
      })
    },
    handleAdd() {   // 新增数据
      this.isEdit = false;
      this.form = {}  // 新增数据的时候清空数据
      this.fromVisible = true   // 打开弹窗
    },
    save() {
      var requestType = this.isEdit ? 'put' : 'post'
      this.$refs.formRef.validate((valid) => {
        if (valid) {
          if(requestType === 'post'){
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
          else{
            this.$request.put('/user/' + this.form.username, this.form).then(res => {
              if (res.message === 'User updated successfully.') {
                this.$message.success('User updated successfully.');
                this.loadTableData();
                this.fromVisible = false;
              } else if (res.detail && res.detail.length > 0) {
                this.$message.error(res.detail[0].msg);
              }
            }).catch(error => {
              this.$message.error('An error occurred while updating the user.');
            });
          }
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
    handleAvatarSuccess(response) {
      this.$message.success(response.message);
      // Ensure the response contains the base64 encoded image
      if (response.annotated_image) {
        this.form.face_image = response.annotated_image;
        console.log(this.form.face_image);
      } else {
        this.$message.error('Failed to retrieve the image data');
      }
    },
    beforeUpload(file) {
      const isImage = file.type.startsWith('image/');
      if (!isImage) {
        this.$message.error('The uploaded file must be an image');
      }
      return isImage;
    },
  }
}
</script>

<style scoped>

</style>
