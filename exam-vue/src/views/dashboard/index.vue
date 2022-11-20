<template>
  <div class="app-container">
    <div id="cnblogs_post_body" class="blogpost-body blogpost-body-html">
      <h1>考试管理系统<button class="cnblogs-toc-button" title="显示目录导航" aria-expanded="false"></button></h1>
      <h2>项目演示<button class="cnblogs-toc-button" title="显示目录导航" aria-expanded="false"></button></h2>
      <p>管理账号：admin/admin <br>学员账号：person/person   </p>
      <h2>介绍<button class="cnblogs-toc-button" title="显示目录导航" aria-expanded="false"></button></h2>
      <p>一款多用户在线培训考试系统，系统集成了用户管理、小组管理、题库管理、试题管理、试题导入导出、考试管理、在线考试、错题训练等功能，考试流程完善。</p>
      <h2>技术栈<button class="cnblogs-toc-button" title="显示目录导航" aria-expanded="false"></button></h2>
      <p><em id="__mceDel">SpringBoot / Shiro / Vue / MySQL</em></p>
      <h2>产品功能<button class="cnblogs-toc-button" title="显示目录导航" aria-expanded="false"></button></h2>
      <p>系统完善：完善的权限控制和用户系统<br>权限控制：基于Shiro和JWT开发的权限控制功能。    <br>用户系统：用户管理、小组管理等。    </p>
      <p>多用户：多用户支持    <br>考试端：学生用户、支持在线考试、查看分数、训练错题。    <br>管理端：题库管理、试题管理、考试管理、用户小组管理、查看考试情况等等。    </p>
      <p>考试权限：考试权限定义<br>完全公开：任何人员都可以参与考试。    <br>指定小组：只有选中小组的人员才可以看到考试。    </p>
      <p>多题型：常用题型支持    <br>支持题型：单选题、多选题、判断题。    <br>难易程度：普通、困难。    </p>
      <p>便捷组卷：题库组卷    <br>题库组卷：指定题库、分数、数量；题目、选项随机排序</p>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'Dashboard',
  data() {
    return {
      currentRole: 'adminDashboard'
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
