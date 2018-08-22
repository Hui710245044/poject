<template>
  <div>
    状态:
    <el-radio-group v-model="radio3" size="mini" @change="filerate">
      <el-radio-button label="">全部</el-radio-button>
      <el-radio-button :label="true" style="margin-left:10px">已启用</el-radio-button>
      <el-radio-button :label="false" style="margin-left:10px">已禁用</el-radio-button>
    </el-radio-group>


    授权状态:
    <el-select v-model="value" placeholder="全部" style="width:110px" size="mini">
      <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
      </el-option>
    </el-select>

  </div>
</template>
<script>
    import event from '../enent.js'
    export default {
        data() {
            return {
                radio3: '全部',
                options: [{
                    value: '选项1',
                    label: '已授权'
                }, {
                    value: '选项2',
                    label: '未授权'
                }, {
                    value: '选项5',
                    label: '速卖通'
                }],
                value: ''
            }
        },
        methods: {
            filerate(radio3){
                this.$emit('filerate333',this.radio3)
            }
        }
    }
</script>
<style>
    * {
        padding: 0;
        margin: 0;
    }
    
    .header-pannel {
        width: auto;
        margin-top: 10px;
    }
</style>