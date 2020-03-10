<template>
  <div class="app-container">
    <div class="app-container-head">
      资源管理
    </div>
    <div class="app-container-content">
      <!-- <div class="table-tool">
            <el-button type="primary" icon="el-icon-plus" @click="dialog(true)" plain>添加</el-button>
        </div> -->
      <div class="table-search">
        <div class="search-content">
          <div class="search-item">
            <span class="search-label">资源名称：</span>
            <span class="search-input">
              <el-input v-model="input" placeholder="请输入内容" />
            </span>
          </div>
          <div class="search-btn">
            <el-button type="primary" icon="el-icon-search">搜索</el-button>
            <span class="search-clear">清空选择条件</span>
          </div>
        </div>
      </div>
      <div class="table-main">
        <el-table :data="tableData" border style="width: 100%">
          <el-table-column prop="name" label="资源名称" />
          <el-table-column prop="name" label="路径" />
          <el-table-column prop="province" label="分类" width="150" />
          <el-table-column prop="date" label="状态" width="60" />
          <el-table-column fixed="right" label="操作" width="100">
            <template slot-scope="scope">
              <el-button type="text" size="small" @click="editApi(scope.row)">编辑</el-button>
              <!-- <el-button @click="handleClick(scope.row)" type="text" size="small">删除</el-button> -->
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div class="table-page">
        <el-pagination background layout="total, sizes, prev, pager, next, jumper" :total="4" />
      </div>
    </div>
    <div class="Api-detail-dialog">
      <el-dialog :visible.sync="editApiDlgVis" title="编辑Api" center="true" width="45%" top="3vh">
        <edit-api />
      </el-dialog>
    </div>
  </div>
</template>

<script>
import { EditApi } from './components'
export default {
  components: { EditApi },
  data() {
    return {
      editApiDlgVis: false,
      tableData: [{
        name: '河南锦弘基石数据科技有限公司'
      }],
      options: [{
        value: '选项1',
        label: '选项1'
      }, {
        value: '选项2',
        label: '选项2'
      }, {
        value: '选项3',
        label: '选项3'
      }],
      value: '',
      pickerOptions: {
        shortcuts: [{
          text: '最近一周',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近一个月',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近三个月',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
            picker.$emit('pick', [start, end])
          }
        }]
      },
      value1: '',
      value2: ''
    }
  },
  methods: {
    editApi(row) {
      this.editApiDlgVis = true
    }
  }
}

</script>

<style lang="scss" scoped>

</style>
