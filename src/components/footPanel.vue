<template>
  <div>
    <el-select v-model="value" placeholder="店铺名" style="width:110px" size="mini">
      <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
      </el-option>
    </el-select>

    <el-select v-model="input" @click="search" size="mini" style="width:140px" multiple filterable remote reserve-keyword placeholder="请输入简称..." :remote-method="remoteMethod"
      :loading="loading">
      <el-option v-for="item in options4" :key="item.value" :label="item.label" :value="item.value">
      </el-option>
    </el-select>
    <el-button type="primary" size="mini"> 搜索</el-button>
    <el-button size="small">还原修改</el-button>
    <el-button size="mini" type="warning">清空条件</el-button>
    
    
    
  </div>
</template>
<script>
  export default {
    data() {
      return {
          input:'',
        options: [{
          value: '选项1',
          label: 'ebay'
        }, {
          value: '选项4',
          label: '亚马逊'
        }, {
          value: '选项5',
          label: '速卖通'
        }],
        value: '',
        options4: [],
        value9: [],
        list: [],
        loading: false,
        states: ["eb","yama","sumai"]
      }
    },
    mounted() {
      this.list = this.states.map(item => {
        return {
          value: item,
          label: item
        };
      });
    },
    methods: {
        search(input){
            this.$emit('search11', this.input)
        },
      remoteMethod(query) {
        if (query !== '') {
          this.loading = true;
          setTimeout(() => {
            this.loading = false;
            this.options4 = this.list.filter(item => {
              return item.label.toLowerCase()
                .indexOf(query.toLowerCase()) > -1;
            });
          }, 200);
        } else {
          this.options4 = [];
        }
      }
    }
  }

</script>
