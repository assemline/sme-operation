<template>
  <div class="menu-container clearfix">
    <div class="tree-nav f-left">
      <div class="tree-search">
        <el-form :inline="true" :model="menuSearch" class="menu-search-form">
          <el-form-item style="width:63%;margin-left:10px;">
            <el-input v-model="filterText" placeholder="请输入关键字" />
          </el-form-item>
          <el-form-item>
            <el-button type="primary" icon="el-icon-search" class="search-btn" plain @click="searchTree" />
          </el-form-item>
        </el-form>
      </div>
      <div class="tree-nav-content">
        <!-- <el-tree :data="data" :props="defaultProps" @node-click="handleNodeClick" /> -->
        <el-tree
          ref="tree"
          :data="data"
          show-checkbox
          node-key="id"
          default-expand-all
          :expand-on-click-node="false"
          :filter-node-method="filterNode"
        >
          <span slot-scope="{ node, data }" class="custom-tree-node">
            <span>{{ node.label }}</span>
            <span>
              <el-button
                type="text"
                size="mini"
                @click="() => append(data)"
              >
                编辑
              </el-button>
              <el-button
                type="text"
                size="mini"
                @click="() => remove(node, data)"
              >
                删除
              </el-button>
            </span>
          </span>
        </el-tree>
      </div>
    </div>
    <div class="menu-content f-left">
      <div class="content-head">
        菜单信息
      </div>
      <div class="content-form">
        <el-form ref="ruleForm" :model="ruleForm" :rules="rules" label-width="100px" class="demo-ruleForm">
          <el-form-item label="上级菜单" prop="name">
            <el-input v-model="ruleForm.name" />
          </el-form-item>
          <el-form-item label="菜单名称" prop="name">
            <el-input v-model="ruleForm.name" />
          </el-form-item>
          <el-form-item label="菜单路径" prop="name">
            <el-input v-model="ruleForm.name" />
          </el-form-item>
          <el-form-item label="图标" prop="name">
            <el-input v-model="ruleForm.name" />
          </el-form-item>
          <el-form-item label="状态" prop="resource">
            <el-radio-group v-model="ruleForm.resource" size="small">
              <el-radio-button label="启用" />
              <el-radio-button label="禁用" />
            </el-radio-group>
          </el-form-item>
          <el-form-item label="描述" prop="desc">
            <el-input v-model="ruleForm.desc" type="textarea" />
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
            <el-button @click="resetForm('ruleForm')">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
    <div class="action-content f-left">
      <div class="action-search">
        <el-form :inline="true" :model="actionSearch">
          <el-form-item>
            <el-input v-model="actionSearch.actionName" placeholder="请输入关键字" />
          </el-form-item>
          <el-form-item>
            <el-button type="primary" icon="el-icon-search" class="search-btn" plain @click="saerchAction()" />
          </el-form-item>
          <el-form-item>
            <el-button type="primary" icon="el-icon-plus" class="search-btn" plain @click="addAction(true)" />
          </el-form-item>
        </el-form>
      </div>
      <div class="action-table">
        <el-table :data="tableData" border style="width: 100%">
          <el-table-column prop="name" label="功能名称" />
          <el-table-column prop="name" label="编码" />
          <el-table-column fixed="right" label="操作" width="100">
            <template slot-scope="scope">
              <el-button type="text" size="small" @click="editAction(scope.row)">编辑</el-button>
              <el-button type="text" size="small" @click="handleClick(scope.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div class="table-page">
        <el-pagination background layout="total, sizes, prev, pager, next" :total="4" />
      </div>
    </div>
    <div class="Role-detail-dialog">
      <el-dialog :visible.sync="addActionDlgVis" title="新增角色" center="true" width="45%" top="3vh">
        <add-action />
      </el-dialog>
    </div>
    <div class="Role-detail-dialog">
      <el-dialog :visible.sync="editActionDlgVis" title="编辑角色" center="true" width="45%" top="3vh">
        <edit-action />
      </el-dialog>
    </div>
  </div>
</template>

<script>
import { AddAction, EditAction } from './components'

let id = 1000

export default {
  components: { AddAction, EditAction },
  data() {
    return {
      addActionDlgVis: false,
      editActionDlgVis: false,
      data: [{
        label: '系统管理',
        children: [{
          label: '用户管理',
          id: '1'
        }, {
          label: '角色管理',
          id: '2'
        }, {
          label: '菜单管理',
          id: '3'
        }, {
          label: '资源管理',
          id: '4'
        }]
      }, {
        label: '参数配置',
        id: '5'
      }],
      defaultProps: {
        children: 'children',
        label: 'label'
      },
      filterText: '',
      ruleForm: {
        name: '',
        resource: '启用',
        desc: ''
      },
      rules: {
        name: [
          { required: true, message: '请输入活动名称', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ]
      },
      menuSearch: {
        filterText: ''
      },
      actionSearch: {
        actionName: ''
      }
    }
  },
  methods: {
    append(data) {
      const newChild = { id: id++, label: 'testtest', children: [] }
      if (!data.children) {
        this.$set(data, 'children', [])
      }
      data.children.push(newChild)
    },

    remove(node, data) {
      const parent = node.parent
      const children = parent.data.children || parent.data
      const index = children.findIndex(d => d.id === data.id)
      children.splice(index, 1)
    },

    filterNode(value, data) {
      if (!value) return true
      return data.label.indexOf(value) !== -1
    },

    searchTree() {
      this.$refs.tree.filter(this.filterText)
    },

    addAction(val) {
      this.addActionDlgVis = val
    },

    editAction(row) {
      this.editActionDlgVis = true
    },

    saerchAction(){
      
    }
  }
}

</script>

<style lang="scss">
.custom-tree-node {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 14px;
    margin-right: 10px;
  }

.menu-search-form,.content-form, .action-search{
  .el-input__inner{
    height: 35px;
  }
}
</style>

<style lang="scss" scoped>
.menu-container{
  padding:20px;
  min-height: calc(100vh - 144px);
}
.tree-nav{
    background:#fff;
    min-height: calc(100vh - 144px);
    width:300px;
    padding:10px 6px;
}
.search-btn{
  height: 35px;
  padding:10px 20px;
}

.tree-nav-content{
    min-height: 100%;
    width:100%;
}
.menu-content{
  width:calc(100vw - 1040px);
  background:#fff;
  margin-left: 20px;
}
.content-head{
  border-bottom: 1px solid #eee;
  height: 35px;
  line-height: 35px;
  padding-left: 15px;
  margin:5px 0px 15px;
  font-weight: 700;
}
.content-form{
  padding:20px 20px 0px 0px;
}
.action-content{
  background:#fff;
  margin-left: 20px;
  width:400px;
  min-height: 100%;
  padding:20px;
}
</style>

