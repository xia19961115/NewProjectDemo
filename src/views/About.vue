<template>
  <div>
    <ysn-table
      :data="data"
      :options="options"
      :selection="true"
      @edit="handleEdit"
    >
      <template v-slot:slot='scope'>
        <div>{{scope.name+'插槽中的数据'}}</div>
      </template>
    </ysn-table>
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
          name: "王小虎1",
          address: "上海市普陀区金沙江路 1519 弄",
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
        },
        {
          date: "2016-05-01",
          name: "王小虎2",
          address: "上海市普陀区金沙江路 1519 弄",
          src: "https://img2.baidu.com/it/u=3666548066,2508071679&fm=26&fmt=auto",
        },
      ],
      options: [
        {
          label: "日期",
          prop: "date",
          align: "center",
          width: "100px",
          showOverflowTooltip: true,
        },
        {
          label: "姓名",
          prop: "name",
        },
        {
          label: "地址",
          prop: "address",
        },
        {
          label: "图片",
          type: "image",
          prop: "src",
        },
        {
          label: "插槽",
          type: "slot",
          prop: "slot",
        },
        {
          label: "操作",
          type: "handle",
          child: [
            {
              label: "编辑",
              type: "info",
              prop: "edit",
              myicon: "el-icon-delete",
              size: "mini",
              popconfirm: {
                title: "删除",
              },
              rule: {
                prop: "name",
                value: "王小虎1",
              },
            },
            {
              label: "详情",
              type: "warning",
              prop: "detail",
              size: "mini",
            },
          ],
        },
      ],
    };
  },
  methods: {
    handleEdit(param) {
      console.log(param.date);
    },
    handle() {
      console.log('666666');
    }
  },
};
</script>