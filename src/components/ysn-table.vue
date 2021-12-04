<template>
    <div>
      <el-table
        :data="compileData"
        @selection-change="handleSelection"
      >
      <!-- 是否需要筛选 -->
        <el-table-column
          v-if="selection"
          type="selection"
          width="55">
        </el-table-column>
        <el-table-column
          v-for="(item,index) in options"
          :key="index"
          :prop="item.prop"
          :label="item.label"
          v-bind="{...$attrs,...item}"
        >
         <template slot-scope="scope">
             <div>{{ scope.row[item.prop] }}</div>
         </template>

        </el-table-column>
        
      </el-table>
    </div>
</template>
<script>
export default {
    name:'YsnTable',
    props:{
        data:{
            type: Array,
            default:() => []
        },
        options:{
            type: Array,
            default:() => []
        },
        selection:{
            typeof:Boolean,
            default:false
        }
    },
    data(){
        return{

        }
    },
    computed: {
      compileData() {
          return this.data
      }  
    },
    methods:{
        handleSelection(param) {
            this.$emit('selection', param)
        }
    }
}
</script>