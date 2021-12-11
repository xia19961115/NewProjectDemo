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
              @change="item.change && item.change(form[item.prop])"
            ></el-input>
          </template>
          <!-- 下拉框 -->
          <template v-if="item.mytype === 'select'">
            <div class="search-font">{{ item.label }}：</div>
            <el-select
              class="search-flex1"
              v-model="form[item.prop]"
              v-bind="{ ...item }"
              @change="item.change && item.change(form[item.prop])"
            >
              <el-option
                v-for="item in item.list"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </template>
          <!-- time -->
          <template v-if="item.mytype === 'time'">
            <div class="search-font">{{ item.label }}：</div>
            <el-time-picker
              class="search-flex1"
              is-range
              arrow-control
              v-model="form[item.prop]"
              range-separator="至"
              start-placeholder="开始时间"
              end-placeholder="结束时间"
              placeholder="选择时间范围"
              v-bind="{ ...item }"
            ></el-time-picker>
          </template>
          <!-- date -->
          <template v-if="item.mytype === 'date'">
            <div class="search-font">{{ item.label }}：</div>
            <el-date-picker
              class="search-flex1"
              v-model="form[item.prop]"
              v-bind="{...item}"
            ></el-date-picker>
          </template>
          <!-- dateTime -->
          <template v-if="item.mytype==='dateTime'">
            <div class="search-font">{{ item.label }}：</div>
            <el-date-picker
            class="search-flex1"
            v-model="form[item.prop]"
            v-bind="{...item}"
            ></el-date-picker>
          </template>
          <!-- 按钮 -->
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
  created() {
    console.log(this.$attrs)
    console.log(this.$listeners);
  },
  data() {
    return {
      span: {
        input: 6,
        select: 6,
        time: 6,
        date:6,
        button: 3,
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
  width: 100%;
  display: flex;
  align-items: center;
  .el-row {
    width: 100%;
  }
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