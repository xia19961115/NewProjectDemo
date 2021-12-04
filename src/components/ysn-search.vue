<template>
  <div class="search">
    <el-row :gutter="20">
      <el-col
        v-for="(item, index) in options"
        :key="index"
        :span="span[item.mytype]"
      >
        <div class="search-content">
            <!-- 输入框 -->
          <template v-if="item.mytype === 'input'">
            <div class="search-font">{{ item.label }}：</div>
            <el-input
              class="search-flex1"
              v-model="form[item.prop]"
              v-bind="{ ...item }"
            ></el-input>
          </template>
          <!-- 下拉框 -->
          <template v-if="item.mytype==='select'">
            <div class="search-font">{{ item.label }}：</div>
            <el-select class="search-flex1" v-model="form[item.prop]" v-bind="{...item}">
              <el-option
                v-for="item in item.list"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </template>
          <template v-if="item.mytype === 'button'">
            <el-button
              v-bind="{ ...item }"
              @click="handleEmit(item.prop)"
              class="search-flex1"
              >{{ item.label }}</el-button
            >
          </template>
        </div>
      </el-col>
    </el-row>
  </div>
</template>
<script>
export default {
  name: "ysnSearch",
  props: {
    options: {
      typeof: Array,
      default: () => [],
    },
  },
  data() {
    return {
      span: {
        input: 6,
        select:6,
        button:3
      },
      form: {},
    };
  },
  methods: {
    handleEmit(emit) {
      this.$emit(emit, this.form);
    },
  },
};
</script>
<style scoped lang="scss">
.search {
  &-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: 20px;
  }
  &-font {
    font-size: 14px;
  }
  &-flex1 {
    flex: 1;
  }
}
</style>