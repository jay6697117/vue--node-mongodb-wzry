<template>
  <div>
    <h1>模块列表</h1>
    <div>
      <el-button type="primary" size="small" @click='$router.push("/models/create")' style="margin-bottom:.5rem;"><i
          class="el-icon-plus"></i>添加</el-button>
    </div>
    <el-table :data="items" border stripe>
      <el-table-column prop="_id" label="ID" width="230"></el-table-column>
      <el-table-column prop="parent.name" label="上级模块"></el-table-column>
      <el-table-column prop="name" label="模块名称"></el-table-column>
      <el-table-column fixed="right" label="操作" width="180">
        <template slot-scope="scope">
          <el-button type="primary" icon="el-icon-edit" size="small" circle
            @click="$router.push(`/models/edit/${scope.row._id}`)"></el-button>
          <el-button type="danger" icon="el-icon-delete" size="small" circle @click="remove(scope.row)"></el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        items: []
      }
    },
    methods: {
      // 获取列表
      async fetch() {
        const res = await this.$http.get('rest/models');
        this.items = res.data;
      },
      // 删除
      async remove(row) {
        this.$confirm(`是否删除模块"${row.name}"`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(async () => {
          await this.$http.delete(`rest/models/${row._id}`)
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          this.fetch(); // 刷新列表
        }).catch(() => {

        });
      }
    },
    created() {
      this.fetch(); // 在列表组件渲染成功后自动执行该方法获取数据库数据
    }
  }
</script>
<style lang='less' scoped>
</style>