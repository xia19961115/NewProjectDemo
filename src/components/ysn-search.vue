<template>
  <div class="search">
    <div>
      <!-- 非下拉展示 -->
      <div class="search-isFirst">
        <div  v-for="(item, index) in options" :key="index">
          <div :class="item.isFirst || item.mytype==='button'? '' : 'search-none'">
            <template v-if="item.mytype === 'input'">
              <div class="search-isFirst-item">
                <div class="search-font">{{ item.label }}：</div>
                <el-input
                  class="search-flex1"
                  v-model="form[item.prop]"
                  v-bind="{ ...item }"
                  @change="item.change && item.change(form[item.prop])"
                ></el-input>
              </div>
            </template>
          <!-- 下拉框 -->
          <template v-if="item.mytype === 'select'">
            <div class="search-isFirst-item">
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
            </div>
          </template>
          <!-- time -->
          <template v-if="item.mytype === 'time'">
            <div class="search-isFirst-item">
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
            </div>
          </template>
          <!-- date -->
          <template v-if="item.mytype === 'date'">
            <div class="search-isFirst-item">
              <div class="search-font">{{ item.label }}：</div>
              <el-date-picker
                class="search-flex1"
                v-model="form[item.prop]"
                v-bind="{...item}"
              ></el-date-picker>
            </div>
          </template>
          <!-- dateTime -->
          <template v-if="item.mytype==='dateTime'">
            <div>
              <div class="search-font">{{ item.label }}：</div>
              <el-date-picker
              class="search-flex1"
              v-model="form[item.prop]"
              v-bind="{...item}"
              ></el-date-picker>
            </div>
          </template>
          <!-- 按钮 -->
          <template v-if="item.mytype === 'button'">
            <el-button
              style="marginLeft:8px"
              v-bind="{ ...item }"
              @click="handleEmit(item.prop)"
              >{{ item.label }}</el-button
            >
          </template>
          </div>
        </div>
        <el-button type="text" v-if="needShowMore" style="marginLeft:8px" @click="handleReset">重置</el-button>
        <el-button type="text" v-if="needReset" style="marginLeft:8px" @click="handleClick">{{show3?'收起更多' : '下拉更多'}}<i :class="show3 ? 'el-icon-arrow-up' : 'el-icon-arrow-down'"></i></el-button>
      </div>
      <!-- 下拉展示 -->
      <el-collapse-transition>
        <div v-show="show3">
          <div class="search-noneFirst">
            <div  v-for="(item, index) in options" :key="index">
              <div :class="!item.isFirst ? '' : 'search-none'">
              <template v-if="item.mytype === 'input'">
                <div class="search-isFirst-item" style="marginBottom:10px">
                  <div class="search-font">{{ item.label }}：</div>
                  <el-input
                    class="search-flex1"
                    v-model="form[item.prop]"
                    v-bind="{ ...item }"
                    @change="item.change && item.change(form[item.prop])"
                  ></el-input>
                </div>
              </template>
            <!-- 下拉框 -->
            <template v-if="item.mytype === 'select'">
              <div class="search-isFirst-item" style="marginBottom:10px">
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
              </div>
            </template>
            <!-- time -->
            <template v-if="item.mytype === 'time'">
              <div class="search-isFirst-item" style="marginBottom:10px">
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
              </div>
            </template>
            <!-- date -->
            <template v-if="item.mytype === 'date'">
              <div class="search-isFirst-item" style="marginBottom:10px">
                <div class="search-font">{{ item.label }}：</div>
                <el-date-picker
                  class="search-flex1"
                  v-model="form[item.prop]"
                  v-bind="{...item}"
                ></el-date-picker>
              </div>
            </template>
            <!-- dateTime -->
            <template v-if="item.mytype==='dateTime'">
              <div class="search-isFirst-item" style="marginBottom:10px">
                <div class="search-font">{{ item.label }}：</div>
                <el-date-picker
                class="search-flex1"
                v-model="form[item.prop]"
                v-bind="{...item}"
                ></el-date-picker>
              </div>
            </template>
              </div>
            </div>
          </div>
        </div>
      </el-collapse-transition>
    </div>
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
    needShowMore: {
      typeof:Boolean,
      default:false
    },
    needReset: {
      typeof:Boolean,
      default:false
    }
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
      show3:false,
      form: {},
    };
  },
  methods: {
    handleEmit(emit) {
      this.$emit(emit, this.form);
    },
    handleClick() {
      this.show3 = !this.show3
    },
    handleReset() {
      this.form={}
    }
  },
};
</script>
<style scoped lang="scss">
.search{
  width: 100%;
  padding: 20px;
  box-sizing: border-box;
  min-height: 100px;
  display: flex;
  align-items: center;
  &-none{
    display: none !important;
  }
  &-font{
    width:80px
  }
  &-flex1{
    flex:1
  }
  &-showflex{
    display: flex !important;
  }
  &-isFirst{
    width: 100%;
    display: flex;
    margin-bottom: 10px;
    &-item{
      display: flex;
      align-items: center;
      width: 300px;
      margin-left: 8px;
    }
  }
  &-noneFirst{
    display: flex;
    flex-wrap: wrap;
    width: 100%;
  }
}
// .search {
//   width: 100%;
//   display: flex;
//   align-items: center;
//   .el-row {
//     width: 100%;
//   }
//   &-content {
//     display: flex;
//     justify-content: space-between;
//     align-items: center;
//     padding-bottom: 20px;
//   }
//   &-font {
//     font-size: 14px;
//   }
//   &-flex1 {
//     flex: 1;
//   }
// }
</style>