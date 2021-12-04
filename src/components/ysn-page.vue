<template>
  <div>
    <el-pagination
      :background="background"
      layout="total, sizes, prev, pager, next, jumper"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page.sync="page"
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
    background: {
      typeof: Boolean,
      default: false,
    },
  },
  data() {
    return {
      size: 10,
      page:1
    };
  },
  methods: {
    handleSizeChange(val) {
      this.size = val;
      this.$emit("change", this.page, this.size);
    },
    handleCurrentChange(val) {
      this.page = val;
      this.$emit("change", this.page, this.size);
    },
  },
};
</script>