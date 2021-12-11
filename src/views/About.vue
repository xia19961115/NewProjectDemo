<template>
  <div>
    <ysn-search :options="searchOption" @search="handleSearch" @add="handleAdd">
    </ysn-search>
    <ysn-table
      :data="data"
      :options="options"
      :selection="true"
      @edit="handleEdit"
      :row-class-name="tableRowClassName"
    >
      <template v-slot:slot="scope">
        <div>{{ scope.name + "插槽中的数据" }}</div>
      </template>
    </ysn-table>
    <ysn-page
      :total="total"
      :page="page"
      :size="size"
      @change="handlePage"
    ></ysn-page>
  </div>
</template>
<script>
import ysnTable from "@/components/ysn-table.vue";
import ysnPage from "@/components/ysn-page.vue";
import ysnSearch from "@/components/ysn-search.vue";
export default {
  components: { ysnTable, ysnPage, ysnSearch },
  data() {
    return {
      data: [
        {
          date: '2016-05-02',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        },
        {
          date: '2016-05-02',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        },
        {
          date: '2016-05-02',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        },
        {
          date: '2016-05-02',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        },
        {
          date: '2016-05-02',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
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
          sortable: true,
        },
        {
          label: "姓名",
          prop: "name",
        },
        {
          label: "区域",
          prop: "province",
          formatter:(row)=> this.nameListChange(row.province)
        },
        {
          label: "编码",
          prop: "zip",
        },
        {
          label: "地址",
          prop: "address",
          showOverflowTooltip: true,
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
          showOverflowTooltip: true,
        },
        {
          label: "操作",
          type: "handle",
          fixed:"right",
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
      searchOption: [
        {
          mytype: "input",
          label: "搜索",
          prop: "search",
          placeholder: "请输入内容",
          clearable: true,
          change:this.change
        },
        {
          mytype: "select",
          label: "下拉框",
          prop: "select",
          placeholder: "请选择",
          multiple:true,
          clearable: true,
          list: [
            {
              value: 1,
              label: "张三",
            },
          ],
        },
        {
          mytype: "time",
          label: "时间",
          prop: "time",
          valueFormat: "HH:mm:ss",
        },
        {
          mytype: "date",
          label: "日期",
          prop: "date",
          type: "daterange",
          rangeSeparator: "至",
          startPlaceholder: "开始日期",
          endPlaceholder: "结束日期",
          defaultTime: ["00:00:00", "23:59:59"],
          valueFormat: "yyyy-MM-dd HH:mm:ss",
        },
        {
          mytype: "button",
          label: "搜索",
          prop: "search",
          type: "info",
        },
        {
          mytype: "button",
          label: "添加",
          prop: "add",
          type: "primary",
        },
        {
          mytype: "resetButton",
        },
      ],
      page: 1,
      size: 10,
      total: 100,
    };
  },
  methods: {
    handleEdit(param) {
      console.log(param.date);
    },
    handle() {
      console.log("666666");
    },
    handlePage(param) {
      const { page, size } = param;
      this.page = page;
      this.size = size;
    },
    handleSearch(val) {
      console.log(val);
      console.log(this.page);
    },
    handleAdd() {
      console.log("7777");
    },
    tableRowClassName({ row, rowIndex }) {
      if (rowIndex === 1) {
        return "warning-row";
      } else if (rowIndex === 3) {
        return "success-row";
      }
      return "";
    },
    change(val) {
      console.log(val)
      val.select = ''
    },
    nameListChange(val) {
      return val +'牛逼'
    }
  },
};
</script>
<style>
  .el-table .warning-row {
    color: #F00;;
  }

  .el-table .success-row {
    background: #f0f9eb !important;
  }
</style>