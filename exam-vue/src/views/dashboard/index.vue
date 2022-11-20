<template>
  <div class="app-container">
    <el-row :gutter="20">
      <el-col :span="12">
        <el-card class="box">
          <div slot="header" class="clearfix">
            <h1>考试管理系统</h1>
          </div>

          <div style="line-height: 32px;">

            <div class="title" style="padding-top: 0px">系统介绍</div>
            一款多用户在线考试系统，系统集成了用户管理、小组管理、题库管理、试题管理、考试管理、在线考试等功能，操作界面简易，运行流畅。

            <div class="title">技术栈</div>
            <div class="tags">
              <el-tag>SpringBoot</el-tag>
              <el-tag>Shiro</el-tag>
              <el-tag>JWT</el-tag>
              <el-tag>Vue</el-tag>
              <el-tag>MySQL</el-tag>
            </div>

            <div class="title">系统功能</div>
            <div>
              <div>权限控制：基于Shiro和JWT开发的权限控制功能。</div>
              <div>基础功能：系统配置、用户管理、小组管理、权限管理等。</div>
              <div>题库管理：支持单选题、多选题、判断题，支持自动判题</div>
              <div>考试权限：支持指定考试范围：完全公开、指定部分小组考试、试卷随机抽题</div>
              <div>在线考试：学生在线考试、考试计时、查看分数和解析。</div>
            </div>



          </div>

        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card class="box" shadow="hover" style="min-height: 100%;height: 100%;">
          <div class="title">开发成员</div>
          <div>
           游万里、张云飞、朱少本、刘洋、凌好、张康、樊帝冲
          </div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card class="box" shadow="hover" style="min-height: 100%;height: 100%; margin-top: 50px">
          <div class="title">管理员帐号</div>
          <div>
            用户名：admin
            密码：admin
          </div>

          <div class="title">学生帐号</div>
          <div>
            用户名：person
            密码：person
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'
  import config from '../../../package.json'

  export default {
    name: 'Dashboard',
    data() {
      return {
        currentRole: 'adminDashboard',
        version: config.version
      }
    },
    computed: {
      ...mapGetters([
        'roles',
        'siteData'
      ])
    },
    created() {

    }
  }
</script>

<style scoped>
  .box{
    height: calc(100vh - 120px);
    font-size: 14px;
    line-height: 28px;
  }
  .title{
    font-size: 18px;
    font-weight: 700;
    padding-bottom: 10px;
    padding-top: 20px;
    margin-bottom: 10px;
    border-bottom: #eee 1px dotted;
  }

  .tags span{
    margin-right: 10px;
  }

  .box a{
    color: #20a0ff;
  }

  .box a:hover{
    color: #ff0000;
  }
</style>
