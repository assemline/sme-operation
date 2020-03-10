<template>
  <div class="category-nav-item">
    <div class="item-content f-left">
      {{ categroyItem.name }}
    </div>
    <div class="item-tool f-right">
      <span class="item-tool-item">编辑</span>
      <span class="item-tool-item">删除</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    categroyItem: {
      type: Object,
      default: null
    }
  }
}
</script>

<style lang="scss" scoped>
.category-nav-item{
    height: 40px;
    line-height: 40px;
    cursor: pointer;
}
.item-tool-item{
    color:#3578bf;
    margin-right: 10px;
}
</style>
