<template>
  <div>
    <div style="margin-top: 20px;display: flex;justify-content: center">
      <el-input
          placeholder="可以通过学生名称搜索..."
          prefix-icon="el-icon-search"
          v-model="keywords" style="width: 400px" size="small">
      </el-input>
      <el-button type="primary" icon="el-icon-search" size="small" style="margin-left: 3px" @click="searchClick">搜索
      </el-button>
      <el-button type="primary" size="mini" icon="el-icon-plus" @click="userForm">添加学生</el-button>
    </div>

    <div>
      <el-main>
        <el-table
            :data="studentList"
            size="mini"
            stripe
            v-loading="tableLoading"
            style="width: 100%">
          <el-table-column type="selection" align="center" width="55"></el-table-column>
          <el-table-column prop="name" align="center" fixed label="名称" width="150"></el-table-column>
          <el-table-column prop="age" align="left" fixed label="年龄" width="100"></el-table-column>
          <el-table-column prop="gender" align="left" fixed label="性别" width="100"></el-table-column>
          <el-table-column label="" align="right">

            <template slot-scope="scope">
              <el-button
                  size="mini" class="el-icon-edit" type="primary"
                  @click="handleEdit(scope.$index, scope.row)">修改
              </el-button>

              <el-button
                  size="mini"
                  class="el-icon-delete" type="danger"
                  @click="handleDelete(scope.$index, scope.row)">删除
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-main>
    </div>

    <!--添加学生信息表单-->
        <el-form :model="userForm" :rules="rules" ref="addUserForm" style="margin: 0;padding: 0;">
          <div>
            <el-dialog
                :title="dialogTitle"
                style="padding: 0;text-align: center"
                :close-on-click-modal="false"
                :visible.sync="dialogVisible"
                width="300px">
              <el-row>
                <el-col>
                  <el-form-item label="姓名:" prop="name">
                    <el-input v-model="studentForm.name" size="mini" style="width: 150px" placeholder="请输入姓名"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col>
                  <el-form-item label="年龄:" prop="age">
                    <el-input v-model="studentForm.age" size="mini" style="width: 150px"
                              placeholder="请输入用户名"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col>
                  <el-form-item label="性别:" prop="gender">
                    <el-select v-model="studentForm.gender" placeholder="请选择" style="width:150px;" size="mini">
                      <el-option
                          v-for="item in genderOptions"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                      </el-option>
                    </el-select>
                  </el-form-item>
                </el-col>
              </el-row>
              <span slot="footer" class="dialog-footer">
            <el-button size="mini" @click="resetUser">取 消</el-button>
            <el-button size="mini" type="primary" @click="saveUser('addUserForm',userForm.id)">确 定</el-button>
          </span>
            </el-dialog>
          </div>
        </el-form>



    <!--修改学生信息表单-->
    <el-form :model="handleEdit" :rules="rules" ref="editUserForm" style="margin: 0;padding: 0;">
      <div>
        <el-dialog
            :title="dialogTitle"
            style="padding: 0;text-align: center"
            :close-on-click-modal="false"
            :visible.sync="dialogVisible"
            width="300px">
          <el-row>
            <el-col>
              <el-form-item label="姓名:" prop="name">
                <el-input v-model="studentForm.name" size="mini" style="width: 150px" placeholder="请输入姓名"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="年龄:" prop="age">
                <el-input v-model="studentForm.age" size="mini" style="width: 150px"
                          placeholder="请输入用户名"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="性别:" prop="gender">
                <el-select v-model="studentForm.gender" placeholder="请选择" style="width:150px;" size="mini">
                  <el-option
                      v-for="item in genderOptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-row>
          <span slot="footer" class="dialog-footer">
            <el-button size="mini" @click="resetUser">取 消</el-button>
            <el-button size="mini" type="primary" @click="saveUser('addUserForm',userForm.id)">确 定</el-button>
          </span>
        </el-dialog>
      </div>
    </el-form>


  </div>
</template>

<script>
export default {
  name: "worldMap",
  mounted: function () {
    //加载后台数据
    // this.loadStudentsList();
  },
  data() {
    return {
      student: [],
      tableLoading: "",
      keywords: "",
      studentName: "",
      dialogTitle: "",
      dialogVisible: true,
      studentList: [
        {
          name: "test1",
          age: 18,
          gender: "男"
        }, {
          name: "test2",
          age: 19,
          gender: "女"
        }, {
          name: "test3",
          age: 20,
          gender: "男"
        }, {
          name: "test1",
          age: 18,
          gender: "男"
        }, {
          name: "test2",
          age: 19,
          gender: "女"
        }, {
          name: "test3",
          age: 20,
          gender: "男"
        }, {
          name: "test1",
          age: 18,
          gender: "男"
        }, {
          name: "test2",
          age: 19,
          gender: "女"
        }, {
          name: "test3",
          age: 20,
          gender: "男"
        },
      ],
      studentForm: {
        name: "",
        age: "",
        gender: ""
      },
      genderOptions: [
        {
          value: 0,
          label: '男'
        },
        {
          value: 1,
          label: '女'
        }
      ],

    }
  },
  methods: {
    loadStudentsList() {
      //获取后台数据
    },
    searchStudent() {
      //搜索学生信息
    },
    //添加
    userForm() {
      this.dialogTitle = "添加用户";
      this.dialogVisible = true;
    },
    //修改
    handleEdit(){
      this.dialogTitle = "修改用户";
      this.dialogVisible = true;
    },
    // handleEdit(row) {
    //   this.dialogTitle = '修改用户';
    //   this.loading = true;
    //   this.$getRequest('/other/get',{id:row.id})
    //       .then(result =>{
    //         this.loading = false;
    //         this.menuDialogVisible = true;
    //         if(result.data.status == 200){
    //           this.menuForm = result.data.data;
    //         }else{
    //           this.$message({type: 'error', message: result.data.message})
    //         }
    //
    //       })
    // },
    //删除
    handleDelete(row) {
      this.$confirm('删除该菜单, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.loading = true;
        this.$deleteRequest('/menu/deleteMenus',{id:row.id})
            .then(result =>{
              this.loading = false;
              if(result.data.status == 200){
                this.$message({type: 'success', message: result.data.message})
                this.emptyMenuData();
                this.loadTreeMenus();
                this.loadTableMenus();
              }else{
                this.$message({type: 'error', message: result.data.message})
              }

            })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
    }
  }
}
</script>

<style>

</style>