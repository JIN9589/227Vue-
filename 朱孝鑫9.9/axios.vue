<template>
  <div>
    <el-container style="height: 500px; border: 1px solid #eee">
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-openeds="['1', '3']">
          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-message"></i>导航一
            </template>
            <el-menu-item-group>
              <template slot="title">分组一</template>
              <el-menu-item index="1-1">选项1</el-menu-item>
              <el-menu-item index="1-2">选项2</el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="分组2">
              <el-menu-item index="1-3">选项3</el-menu-item>
            </el-menu-item-group>
            <el-submenu index="1-4">
              <template slot="title">选项4</template>
              <el-menu-item index="1-4-1">选项4-1</el-menu-item>
            </el-submenu>
          </el-submenu>
          <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-menu"></i>导航二
            </template>
            <el-menu-item-group>
              <template slot="title">分组一</template>
              <el-menu-item index="2-1">选项1</el-menu-item>
              <el-menu-item index="2-2">选项2</el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="分组2">
              <el-menu-item index="2-3">选项3</el-menu-item>
            </el-menu-item-group>
            <el-submenu index="2-4">
              <template slot="title">选项4</template>
              <el-menu-item index="2-4-1">选项4-1</el-menu-item>
            </el-submenu>
          </el-submenu>
          <el-submenu index="3">
            <template slot="title">
              <i class="el-icon-setting"></i>导航三
            </template>
            <el-menu-item-group>
              <template slot="title">分组一</template>
              <el-menu-item index="3-1">选项1</el-menu-item>
              <el-menu-item index="3-2">选项2</el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="分组2">
              <el-menu-item index="3-3">选项3</el-menu-item>
            </el-menu-item-group>
            <el-submenu index="3-4">
              <template slot="title">选项4</template>
              <el-menu-item index="3-4-1">选项4-1</el-menu-item>
            </el-submenu>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-container>
        <el-header style="text-align: right; font-size: 12px">
          <el-dropdown>
            <i class="el-icon-setting" style="margin-right: 15px"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>查看</el-dropdown-item>
              <el-dropdown-item>新增</el-dropdown-item>
              <el-dropdown-item>删除</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
          <span>王小虎</span>
        </el-header>

        <el-main>
          <el-button type="success" @click="checkadd">添加</el-button>
          <el-table
            :data="student.filter(data => !search || data.stu_name.includes(search.toLowerCase()))"
            style="width: 100%"
            v-load="loading"
          >
            <el-table-column label="ID" prop="stu_id"></el-table-column>
            <el-table-column label="姓名" prop="stu_name"></el-table-column>
            <el-table-column label="性别" prop="stu_sex"></el-table-column>
            <el-table-column label="出生日期" prop="stu_birth"></el-table-column>
            <el-table-column align="right">
              <!-- eslint-disable-next-line -->
              <template slot="header" slot-scope="scope">
                <el-input v-model="search" size="mini" placeholder="输入关键字搜索" @input="changeInput" />
              </template>
              <template slot-scope="scope">
                <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">Edit</el-button>
                <el-button
                  size="mini"
                  type="danger"
                  @click="handleDelete(scope.$index, scope.row)"
                >Delete</el-button>
              </template>
            </el-table-column>
          </el-table>

          <el-pagination :page-size="3" :pager-count="5" layout="prev, pager, next" :total="5"></el-pagination>
        </el-main>
        <el-footer></el-footer>
      </el-container>
    </el-container>
    <template>
      <div>
        <el-dialog :title="checktitle" :visible.sync="dialogVisible" width="30%">
          <el-form ref="form" :model="form" label-width="80px">
            <el-form-item label="学号" v-if="isshowid">
              <el-input v-model="editstudent.ID" @input="changeInput" v-if="isshowid"></el-input>
            </el-form-item>
            <el-form-item label="姓名">
              <el-input v-model="editstudent.name" @input="changeInput"></el-input>
            </el-form-item>
            <el-form-item label="性别">
              <el-input v-model="editstudent.sex" @input="changeInput"></el-input>
            </el-form-item>
            <el-form-item label="生日">
              <el-input v-model="editstudent.birth" @input="changeInput"></el-input>
            </el-form-item>
          </el-form>
          <span slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="update(editstudent.ID)" v-if="isupdate">更 新</el-button>
            <el-button type="primary" @click="insert()" v-if="isadd">插 入</el-button>
          </span>
        </el-dialog>
      </div>
    </template>
  </div>
</template>
<script>
export default {
  mounted() {
    this.getdata();
  },
  data() {
    return {
      checktitle:"",
      isshowid: false,
      //显示添加按钮
      isadd: false,
      //显示更新按钮
      isupdate: false,
      student: [],
      editstudent: {},
      search: "",
      dialogVisible: false,
      form: {
        name: "",
        number: ""
      },
      loading: true
    };
  },
  methods: {
    //数据过滤
    
    //数据请求
    getdata() {
      this.student = [];
      this.$axios
        .get("api/getdata")
        .then(response => {
          for (let i = 0; i < response.data.length; i++) {
            let obj = {};
            obj.stu_id = response.data[i].stu_id;
            obj.stu_name = response.data[i].stu_name;
            obj.stu_sex = response.data[i].stu_sex;
            obj.stu_birth = this.changedate(response.data[i].stu_birth);
            this.student.push(obj);
          }
        })
        .catch(err => {
          console.log(err);
          this.msg(err.message);
        });
    },
    //点击编辑按钮
    handleEdit(index, row) {
      this.isshowid = true;
      this.isadd = false;
      this.isupdate = true;
      this.editstudent.ID = row.stu_id;
      this.editstudent.name = row.stu_name;
      this.editstudent.sex = row.stu_sex;
      this.editstudent.birth = this.changedate(row.stu_birth);
      this.dialogVisible = true;
      this.checktitle="修改学生信息"
      // this.open();
    },
    changeInput() {
      this.$forceUpdate();
    },
    //点击确定按钮
    update() {
      this.$axios
        .get("http://localhost:4567/update", {
          params: {
            stu_id: this.editstudent.ID,
            stuname: this.editstudent.name,
            stu_sex: this.editstudent.sex,
            stu_birth: this.editstudent.birth
          }
        })
        .then(response => {
          console.log(response);
          if (
            response.data.code == 200 &&
            response.data.message == "更新成功"
          ) {
            console.log(111);
            this.msg(response.data.message);
            this.getdata();
          } else {
            this.msg(response.message);
          }
          console.log(response);
        })
        .catch(err => {
          this.msg(err);
        });
      this.dialogVisible = false;
    },
    //删除
    handleDelete(index, row) {
      let stu_id = row.stu_id;
      this.$axios
        .get("http://localhost:4567/deldata", {
          params: {
            stu_id: stu_id
          }
        })
        .then(response => {
          if (response.data.code == 200) {
            this.msg("删除成功");
            this.getdata();
          }
        })
        .catch(err => {
          this.msg(err.message);
        });
    },
    //切换到添加
    checkadd() {
      this.dialogVisible = true;
      this.editstudent = [];
      this.isadd = true;
      this.isupdate = false;
      this.isshowid = false;
      this.checktitle="添加学生";
    },
    //添加
    insert() {
      this.dialogVisible = false;
      this.$axios("http://localhost:4567/insert", {
        params: {
          stu_name: this.editstudent.name,
          stu_sex: this.editstudent.sex,
          stu_birth: this.editstudent.birth
        }
      })
        .then(response => {
          this.msg(response.data.message);
          this.getdata();
        })
        .catch(err => {
          this.msg(err.data.message);
        });
    },
    //提示框
    msg(msg) {
      this.$message({
        message: msg,
        center: true
      });
    },
    //改变日期格式
    changedate(date) {
      var mydate = new Date(date);
      var year = mydate.getFullYear();
      var month = mydate.getMonth() + 1; //获取当前月份(0-11,0代表1月)
      var day = mydate.getDate(); //获取当前日(1-31)
      var birth = `${year}-${month}-${day}`;
      return birth;
    }
  }
};
</script>
<style scoped>
.table {
  /* width: 542px; */
  text-align: center;
  margin: 0 auto;
}
.cell {
  text-align: center;
}
</style>
