<template>

  <div>

    <data-table
      ref="pagingTable"
      :options="options"
      :list-query="listQuery"
      @multi-actions="handleMultiAction"
    >

      <template slot="filter-content">

        <el-input v-model="listQuery.params.userName" style="width: 200px" placeholder="搜索用户名" class="filter-item" />
        <el-input v-model="listQuery.params.realName" style="width: 200px" placeholder="搜索姓名" class="filter-item" />

        <el-button class="filter-item" type="primary" icon="el-icon-plus" @click="handleAdd">
          添加
        </el-button>

      </template>

      <template slot="data-columns">

        <el-table-column
          type="selection"
          width="55"
        />

        <el-table-column
          align="center"
          label="用户名"
        >
          <template slot-scope="scope">
            <a @click="handleUpdate(scope.row)">{{ scope.row.userName }}</a>
          </template>

        </el-table-column>

        <el-table-column
          align="center"
          label="姓名"
          prop="realName"
        />

        <el-table-column
          align="center"
          label="权限"
          prop="roleIds"
        />

        <el-table-column
          align="center"
          label="创建时间"
          prop="createTime"
        />

        <el-table-column
          align="center"
          label="状态"
        >

          <template slot-scope="scope">
            {{ scope.row.state | stateFilter }}
          </template>
        </el-table-column>
        <el-table-column
            align="center"
            label="操作"
        >
          <template slot-scope="scope">
            <button type="button" class="el-button el-button--primary el-button--mini" @click="handleEdit(scope.$index, scope.row)">
              <i class="el-icon-edit"></i><span>修改</span></button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)"><i class="el-icon-delete"></i><span>删除</span></el-button>
          </template>
        </el-table-column>

      </template>
    </data-table>

    <el-dialog :visible.sync="dialogVisible" title="添加用户" width="500px">

      <el-form :model="formData" label-position="left" label-width="100px">

        <el-form-item label="用户名">
          <el-input v-model="formData.userName" placeholder="请输入用户名" />
        </el-form-item>

        <el-form-item label="姓名">
          <el-input v-model="formData.realName" placeholder="请输入姓名" />
        </el-form-item>

        <el-form-item label="密码">
          <el-input v-model="formData.password" show-password  placeholder="请输入密码" type="password" />
        </el-form-item>

        <el-form-item label="小组">
          <depart-tree-select v-model="formData.departId" :options="treeData" :props="defaultProps" />
        </el-form-item>

        <el-form-item label="权限">
          <meet-role v-model="formData.roles" />
        </el-form-item>

        <!--        <el-form-item label="头像" prop="avatar">-->

        <!--          <single-upload-->
        <!--            v-model="formData.avatar"-->
        <!--          />-->
        <!--        </el-form-item>-->

      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="handleSave">确 定</el-button>
      </div>

    </el-dialog>
      <el-dialog :visible.sync="dialogVisible1" title="修改用户" width="500px">

        <el-form :model="formData" label-position="left" label-width="100px">

          <el-form-item label="用户名">
            <el-input v-model="formData.userName" />
          </el-form-item>

          <el-form-item label="姓名">
            <el-input v-model="formData.realName" />
          </el-form-item>

          <el-form-item label="密码">
            <el-input v-model="formData.password" show-password placeholder="不修改请留空" type="password" />
          </el-form-item>

          <el-form-item label="小组">
            <depart-tree-select v-model="formData.departId" :options="treeData" :props="defaultProps" />
          </el-form-item>

          <el-form-item label="权限">
            <meet-role v-model="formData.roles" />
          </el-form-item>

          <!--        <el-form-item label="头像" prop="avatar">-->

          <!--          <single-upload-->
          <!--            v-model="formData.avatar"-->
          <!--          />-->
          <!--        </el-form-item>-->

        </el-form>

        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible1 = false">取 消</el-button>
          <el-button type="primary" @click="handleSave">确 定</el-button>
        </div>

      </el-dialog>

  </div>

</template>

<script>
import DataTable from '@/components/DataTable'
import MeetRole from '@/components/MeetRole'
import { saveData } from '@/api/sys/user/user'
import { deleteOne } from '@/api/sys/user/user'
import DepartTreeSelect from '@/components/DepartTreeSelect'
import { fetchTree } from '@/api/sys/depart/depart'

export default {
  name: 'SysUserList',
  components: { DepartTreeSelect, DataTable, MeetRole },
  filters: {

    // 订单状态
    userState(value) {
      const map = {
        '0': '正常',
        '1': '禁用'
      }
      return map[value]
    }
  },
  data() {
    return {

      treeData: [],
      defaultProps: {
        value: 'id',
        label: 'deptName',
        children: 'children'
      },
      dialogVisible: false,
      dialogVisible1: false,
      listQuery: {
        current: 1,
        size: 10,
        params: {
        }
      },

      formData: {
        avatar: ''
      },

      options: {
        // 列表请求URL
        listUrl: '/exam/api/sys/user/paging',
        // 启用禁用
        stateUrl: '/sys/user/state',
        deleteUrl: '/exam/api/sys/user/delete',
        // 批量操作列表
        multiActions: [
          {
            value: 'enable',
            label: '启用'
          }, {
            value: 'disable',
            label: '禁用'
          },
          {
            value: 'delete',
            label: '删除'
          }
        ]
      }
    }
  },

  created() {
    fetchTree({}).then(response => {
      this.treeData = response.data
    })
  },

  methods: {

    handleUploadSuccess(response) {
      // 上传图片赋值
      this.formData.avatar = response.data.url
    },

    handleAdd() {
      this.formData = {}
      this.dialogVisible = true
    },
    handleEdit(index,data){
      Object.assign(this.formData,data)
      this.formData.roles = data.roleIds.split(',')
      this.formData.password = null
      this.dialogVisible1=true
    },
    handleDelete(index,data){
      this.$confirm('此操作将删除['+data.userName+'],是否继续', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        deleteOne({ids:[data.id]}).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          })
          this.$refs.pagingTable.getList()
        }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
      });
    },


    handleUpdate(row) {
      this.dialogVisible1 = true
      this.formData = row
      this.formData.roles = row.roleIds.split(',')
      this.formData.password = null

      console.log(JSON.stringify(this.formData))
    },
    handleSave() {
      saveData(this.formData).then(() => {
        this.$message({
          type: 'success',
          message: '用户修改成功!'
        })
        this.dialogVisible = false
        this.$refs.pagingTable.getList()
      })
    },

    // 批量操作监听
    handleMultiAction(obj) {
      if (obj.opt === 'cancel') {
        this.handleCancelOrder(obj.ids)
      }
    }
  }
}
</script>
