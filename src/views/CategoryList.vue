<template>
  <div class="category-list">
    <h1 class="title">分类列表</h1>
    <el-table  :data="items" border>
      <el-table-column   prop="_id"  label="ID" width="350">   </el-table-column>
       <el-table-column   prop="parent.name"  label="上级分类" width="">   </el-table-column>
      <el-table-column   prop="name"  label="分类名称" width="">   </el-table-column>
      <el-table-column   prop="times"  label="时间" width="">   </el-table-column>
      <el-table-column  fixed="right"  label="操作"  width="200">
        <template slot-scope="scope">
          <el-button type="text" size="small"  :title="tit"  @click="$router.push(`/categories/edit/${scope.row._id}`)"> 
            <i class="el-icon-edit" style="font-size:20px"></i>
          </el-button>
          <el-button type="text" size="small"  :title="dele"  @click="handelDelete(scope.row)">
            <i class="el-icon-delete" style="font-size:20px"></i>
          </el-button>
        </template>
      </el-table-column>
    </el-table>

  </div>
</template>

<script>
export default {
  name:"CategoryList",
  data(){
    return {
      tit:'编辑',
      dele:'删除',
      items:[]
    }
  },
  methods:{
    async  fetch(){
      const  res =  await  this.$http.get('rest/categories')
      this.items = res.data
    },
    //删除
    async handelDelete(row) {
        this.$confirm(`是否确定要删除分类"${row.name}"`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        })
        .then(async () => {
          const  res = await this.$http.delete(`rest/categories/${row._id}`)   // eslint-disable-line no-unused-vars
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          //重新获取一下数据
          this.fetch()
        })
    }
  },
  created () {
    this.fetch()
  }
}
</script>

<style scoped>
.category-list .title{
  margin-bottom: 30px;
  padding: 10px 0;
  border-bottom: 1px solid  #e5e5e5;
}
</style>
