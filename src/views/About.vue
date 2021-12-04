<template>
  <div>
    <ysn-table :data="data" :options="options"></ysn-table>
  </div>
</template>
<script>
import ysnTable from "@/components/ysn-table.vue";
export default {
  components: { ysnTable },
  data() {
    return {
      data: [
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1517 弄",
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1519 弄",
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
        },
      ],
      options: [
        {
          label: "日期",
          prop: "date",
          align:'center',
          width:'100px',
          showOverflowTooltip:true
        },
        {
          label: "姓名",
          prop: "name",
          formatter:(row)=>{ return row }
        },
        {
          label: "地址",
          prop: "address",
        },
      ],
    };
  },
};
</script>