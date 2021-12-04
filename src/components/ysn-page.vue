<template>
  <div>
    <el-pagination
      :background="background"
      layout="total, sizes, prev, pager, next, jumper"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="page"
      :page-sizes="[5, 10, 20, 50]"
      :page-size="size"
      :total="total"
    >
    </el-pagination>
  </div>
</template>
<script>
export default {
  name: "YsnPage",
  props: {
    total: {
      typeof: Number,
      required: true,
    },
    page: {
      typeof: Number,
      required: true,
      default:1
    },
    size:{
      typeof:Number,
      default:10
    },
    background: {
      typeof: Boolean,
      default: false,
    },
  },
  methods: {
    handleSizeChange(val) {
      this.$emit("change", {page:this.page,size:val});
    },
    handleCurrentChange(val) {
      this.$emit("change", {page:val,size:this.size});
    },
  },
};
</script>