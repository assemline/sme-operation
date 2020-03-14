<template>
  <div class="app-container">
    <div class="address-search">
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
        <el-form-item label="地区">
          <div class="block">
            <el-cascader
              v-model="value"
              :options="options"
              @change="handleChange"
              filterable
            />
          </div>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">查询</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="address-table" v-show="isAddressChange">
      <div class="table-main">
        <el-table :data="tableData" border style="width: 100%">
          <el-table-column prop="name" label="地区" />
          <el-table-column prop="name" label="编码" />
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
  </div>
</template>

<script>
export default {
  data() {
    return {
      isAddressChange:false,
      value: [],
      options: [{
        value: 'beijing',
        label: '北京市',
        children: [{
          value: 'shixiaqu',
          label: '市辖区',
          children: [{
            value: 'xichengqu',
            label: '西城区'
          }, {
            value: 'dongchengqu',
            label: '东城区'
          }]
        }]
      },
      {
        value: 'henan',
        label: '河南省',
        children: [{
          value: 'zhengzhoushi',
          label: '郑州市',
          children: [{
            value: 'xichengqu',
            label: '西城区'
          }, {
            value: 'dongchengqu',
            label: '东城区'
          }]
        }, {
          value: 'kaifengshi',
          label: '开封市'
        }]
      }, 
      {
        value: 'hubei',
        label: '湖北省',
        children: [{
          value: 'wuhan',
          label: '武汉市',
          children: [{
            value: 'jianghanqu',
            label: '江汉区'
          }, {
            value: 'jianganqu',
            label: '江岸区'
          }]
        }],
      }]
    }
  },
  methods: {
    handleChange(value) {
      console.log(value);
      this.isAddressChange = true;
    },

    onSubmit() {
      console.log('submit!')
    }
  }
}
</script>

<style>

</style>
