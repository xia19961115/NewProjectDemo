<template>
  <div class="reserReguCard">
    <el-table 
    :data="compileData" 
    @selection-change="handleSelection"
    v-bind="$attrs"
    >
      <!-- 是否需要筛选 -->
      <el-table-column v-if="selection" type="selection" width="55">
      </el-table-column>
      <!-- 是否需要序号 -->
      <el-table-column v-if="numberSort" type="index" width="60" label="序号">
      </el-table-column>
      <el-table-column
        v-for="(item, index) in options"
        :key="index"
        :prop="item.prop"
        :label="item.label"
        v-bind="{ ...item }"
      >
        <template slot-scope="scope">
          <!-- 图片 -->
          <template v-if="item.type === 'image'">
            <el-image
              style="width: 100px; height: 50px"
              :src="scope.row[item.prop]"
              :preview-src-list="[scope.row[item.prop]]"
            >
            </el-image>
          </template>
          <!-- 插槽处理其他业务 -->
          <template v-else-if="item.type === 'slot'">
            <slot :name="item.prop" v-bind="scope.row"></slot>
          </template>
          <!-- 非handle 为非操作 -->
          <template v-else-if="item.type !== 'handle'">
            <!-- 日期 -->
            <div v-if="item.format">
              <div v-if="scope.row[item.prop] === null">-</div>
              <!-- 此处直接渲染 后端没处理自己处理 -->
              <div v-else>{{ handleDateFormat(scope.row[item.prop]) }}</div>
            </div>
            <!-- 单字典表转义中文 scope.row[item.prop]为数组/字符串多个逗号隔开****无法使用****-->
            <div v-else-if="item.dictionary">
              {{ item.dictionary[scope.row[item.prop]] }}
            </div>
            <!-- 多字典表转义中文 scope.row[item.prop] 为数组/字符串多个逗号隔开 -->
            <div v-else-if="item.dictionaries">
              {{ handleToAnyString(scope.row[item.prop], item.dictionaries) }}
            </div>
            <!-- 默认渲染数据 -->
            <div v-else>{{ scope.row[item.prop] || "-" }}</div>
          </template>
          <!-- 操作 -->
          <template v-else>
            <div style="display: flex">
              <div v-for="(item, index) in item.child" :key="index">
                <!-- 二次确认 popconfirm需要传递一个对象 -->
                <template v-if="item.popconfirm">
                  <el-popconfirm
                    :title="`确认${item.popconfirm.title || item.label}吗？`"
                    :confirm-button-text="item.popconfirm.confirm || '确认'"
                    :cancel-button-text="item.popconfirm.cancel || '取消'"
                    @confirm="handleEmit(item.prop, scope.row, scope.$index)"
                  >
                    <template v-if="item.rule">
                      <el-button
                        v-if="scope.row[item.rule.prop] === item.rule.value"
                        v-bind="{ ...$attrs, ...item }"
                        @click="handleEmit(item.prop, scope.row, scope.$index)"
                        slot="reference"
                      >
                        <i v-if="item.myicon" :class="item.myicon"></i>
                        <div v-else>{{ item.label }}</div>
                      </el-button>
                    </template>
                    <template v-else>
                      <el-button
                        v-bind="{ ...$attrs, ...item }"
                        @click="handleEmit(item.prop, scope.row, scope.$index)"
                        slot="reference"
                      >
                        <i v-if="item.myicon" :class="item.myicon"></i>
                        <div v-else>{{ item.label }}</div>
                      </el-button>
                    </template>
                  </el-popconfirm>
                </template>
                <!-- 非二次确认 -->
                <template v-else>
                  <!-- 规则判断 rule需要传递一个对象 -->
                  <template v-if="item.rule">
                    <el-button
                      v-if="scope.row[item.rule.prop] === item.rule.value"
                      v-bind="{ ...item }"
                      @click="handleEmit(item.prop, scope.row, scope.$index)"
                    >
                      <!-- 需要图标显示安全传入mymyicon -->
                      <i v-if="item.myicon" :class="item.myicon"></i>
                      <!-- 默认文字 值为item.label -->
                      <div v-else>{{ item.label }}</div>
                    </el-button>
                  </template>
                  <template v-else>
                    <el-button
                      v-bind="{ ...$attrs, ...item }"
                      @click="handleEmit(item.prop, scope.row, scope.$index)"
                    >
                      <i v-if="item.myicon" :class="item.myicon"></i>
                      <div v-else>{{ item.label }}</div>
                    </el-button>
                  </template>
                </template>
              </div>
            </div>
          </template>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  name: "YsnTable",
  props: {
    data: {
      type: Array,
      default: () => [],
    },
    options: {
      type: Array,
      default: () => [],
    },
    selection: {
      typeof: Boolean,
      default: false,
    },
    numberSort: {
      typeof: Boolean,
      default: false,
    },
  },
  data() {
    return {};
  },
  computed: {
    compileData() {
      return this.data;
    },
  },
  methods: {
    handleSelection(param) {
      this.$emit("selection", param);
    },
    handleEmit(emit, item, index) {
      this.$emit(emit, item, index);
    },
    // 多字段转中文
    handleToAnyString(data, list) {
      let newData = data;
      if (typeof data === "string") {
        newData = data.split(",");
      }
      return newData.map((item) => list[item]).join(",");
    },
    // 日期处理
    handleDateFormat(timestamp) {
      const dateTime = new Date(timestamp);
      const YY = dateTime.getFullYear();
      const MM =
        dateTime.getMonth() + 1 < 10
          ? "0" + (dateTime.getMonth() + 1)
          : dateTime.getMonth() + 1;
      const D =
        dateTime.getDate() < 10 ? "0" + dateTime.getDate() : dateTime.getDate();
      const hh =
        dateTime.getHours() < 10
          ? "0" + dateTime.getHours()
          : dateTime.getHours();
      const mm =
        dateTime.getMinutes() < 10
          ? "0" + dateTime.getMinutes()
          : dateTime.getMinutes();
      const ss =
        dateTime.getSeconds() < 10
          ? "0" + dateTime.getSeconds()
          : dateTime.getSeconds();
      return `${YY}-${MM}-${D} ${hh}:${mm}:${ss}`;
    },
  },
};
</script>