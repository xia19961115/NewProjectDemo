<template>
  <div>
    <el-table :data="compileData" @selection-change="handleSelection" border>
      <!-- 是否需要筛选 -->
      <el-table-column v-if="selection" type="selection" width="55">
      </el-table-column>
      <el-table-column
        v-for="(item, index) in options"
        :key="index"
        :prop="item.prop"
        :label="item.label"
        v-bind="{ ...$attrs, ...item }"
      >
        <template slot-scope="scope">
          <!-- 非handle 为非操作 -->
          <template v-if="item.type !== 'handle'">
            <!-- 根据业务场景来做判断 -->
            <!-- 自己加 -->
            <!-- 默认渲染数据 -->
            <div>{{ scope.row[item.prop] }}</div>
          </template>
          <template v-else>
            <div style="display: flex">
              <div v-for="(item, index) in item.child" :key="index">
                <!-- 二次确认 -->
                <template v-if="item.popconfirm">
                  <el-popconfirm
                    :title="`确认${item.label}吗？`"
                    confirm-button-text="确认"
                    cancel-button-text="取消"
                    @confirm="handleEmit(item.prop, scope.row, scope.$index)"
                  >
                    <template v-if="item.rule">
                      <el-button
                        v-if="scope.row[item.rule.prop] === item.rule.value"
                        v-bind="{ ...$attrs, ...item }"
                        @click="handleEmit(item.prop, scope.row, scope.$index)"
                        slot="reference"
                      >
                        {{ item.label }}
                      </el-button>
                    </template>
                    <template v-else>
                      <el-button
                        v-bind="{ ...$attrs, ...item }"
                        @click="handleEmit(item.prop, scope.row, scope.$index)"
                        slot="reference"
                      >
                        {{ item.label }}
                      </el-button>
                    </template>
                  </el-popconfirm>
                </template>
                <!-- 非二次确认 -->
                <template v-else>
                  <!-- 规则判断 -->
                  <template v-if="item.rule">
                    <el-button
                      v-if="scope.row[item.rule.prop] === item.rule.value"
                      v-bind="{ ...$attrs, ...item }"
                      @click="handleEmit(item.prop, scope.row, scope.$index)"
                    >
                      {{ item.label }}
                    </el-button>
                  </template>
                  <template v-else>
                    <el-button
                      v-bind="{ ...$attrs, ...item }"
                      @click="handleEmit(item.prop, scope.row, scope.$index)"
                    >
                      {{ item.label }}
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
  },
};
</script>