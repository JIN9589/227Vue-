<template>
  <div class="home">
      <el-container>
          <el-header>
              <el-row>
                  <el-col :span="6">
                      <h3>W227附属幼儿园管理平台</h3>
                  </el-col>
                  <el-col :span="12"></el-col>
                  <el-col :span="6">
                      <p>
                          吉峙燊
                      </p>
                  </el-col>
              </el-row>
          </el-header>
          <el-container>
              <el-aside width="200px">
                  <LeftMenu/>
              </el-aside>
              <el-main>
                  <router-view></router-view>
              </el-main>
          </el-container>
          <el-footer>
              2020 ©W227附属幼儿园后台管理系统
          </el-footer>
      </el-container>
  </div>
</template>

<script>
import LeftMenu from './../components/LeftMenu'
export default {
  name: 'Home',
  components: {
    LeftMenu
  }
}
</script>

<style scoped>
    .el-header, .el-footer {
        background-color: #0F4E74;
        color: white;
        height: 60px;
    }
    .el-header .el-col{
        height: 60px;
        line-height: 60px;
    }
    .el-header .el-col p{
        text-align: right;
    }
    .el-aside {
        background-color: #D3DCE6;
        color: #333;
        text-align: center;
        line-height: 200px;
    }

    .el-main {
        min-height: 601px;
        background-color: #E9EEF3;
        color: #333;
        text-align: center;
        /*line-height: 160px;*/
    }
    .el-footer{
        font-size: small;
        height: 60px;
        line-height: 60px;
        text-align: center;
    }
    body > .el-container {
        margin-bottom: 40px;
    }
</style>
