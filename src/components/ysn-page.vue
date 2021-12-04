<template>
  <div>
    <el-pagination
      :background="background"
      layout="total, sizes, prev, pager, next, jumper"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page.sync="myPage"
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
    background: {
      typeof: Boolean,
      default: false,
    },
  },
  data() {
    return {
      size: 10,
      myPage:this.page
    };
  },
  methods: {
    handleSizeChange(val) {
      this.size = val;
      this.$emit("change", this.myPage, this.size);
    },
    handleCurrentChange(val) {
      this.myPage = val
      this.$emit("change", this.myPage, this.size);
    },
  },
};
</script>