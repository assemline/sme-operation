<template>
  <div class="param-container clearfix">
    <div class="category-container f-left clearfix">
      <el-button type="primary" icon="el-icon-plus" class="add-btn f-left" plain @click="addCategory(true)" >添加</el-button>
      <div class="categroy-nav">
        <category-nav v-for="(item,i) in categoryList" :key="i" :categroy-item="item" ></category-nav>
      </div>
    </div>
    <div class="f-left item-contain">
      <div class="table-tool">
        <el-button type="primary" icon="el-icon-plus" plain @click="addUser(true)">添加</el-button>
      </div>
      <!-- <div class="table-search">
        <div class="search-content">
          <div class="search-item">
            <span class="search-label">登录账号：</span>
            <span class="search-input">
              <el-input v-model="input" placeholder="请输入内容" />
            </span>
          </div>
          <div class="search-btn">
            <el-button type="primary" icon="el-icon-search">搜索</el-button>
            <span class="search-clear">清空选择条件</span>
          </div>
        </div>
      </div> -->
      <div class="table-main">
        <el-table :data="tableData" border style="width: 100%">
          <el-table-column prop="name" label="参数名称" />
          <el-table-column prop="name" label="参数编码" />
          <el-table-column prop="date" label="状态" width="60" />
          <el-table-column fixed="right" label="操作" width="100">
            <template slot-scope="scope">
              <el-button type="text" size="small" @click="editUser(scope.row)">编辑</el-button>
              <el-button type="text" size="small" @click="handleClick(scope.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div class="table-page">
        <el-pagination background layout="total, sizes, prev, pager, next, jumper" :total="4" />
      </div>
    </div>
  </div>
</template>

<script>
import { CategoryNav } from './components'

export default {
  components: { CategoryNav },
  data() {
    return {
      categoryList: [{
        'name': '分类一'
      },{
        'name': '分类二'
      }],
      tableData:[]
    }
  }
}
</script>

<style lang="scss">

</style>

<style lang="scss" scoped>
.category-container{
  width:300px;
  min-height: calc(100vh - 144px);
  background:#fff;
  padding:10px;
}
.add-btn{
  height: 35px;
  padding:10px 20px;
  margin-left:20px;
}
.categroy-nav{
  margin:45px 20px 10px;
}
.item-contain{
  background:#fff;
  padding:0px 20px 0px;
  margin-left: 20px;
  width:calc(100vw - 610px);
  min-height: calc(100vh - 144px);
}
</style>
