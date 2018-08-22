<template>
  <div id="app">
      <el-card class="header-pannel">
        <headerPanel @filerate333="filerate"></headerPanel>
        <br>
        <footPanel @search11="search"></footPanel>
      </el-card>
      <tableShow :tableData="comtableData"></tableShow>

  </div>
  
</template>

<script>

import headerPanel from './components/headerPanel'
import footPanel from './components/footPanel'
import tableShow from './components/tableShow'

export default {
    name: 'App',
    data(){
        return {
            curStatus:'',
            tableData: [{
                shop: 'ebay',
                shopShort: 'eb',
                id: 'gangsate',
                status: true,
                idShort: 'a'
            }, {
                account: 'a',
                shop: '亚马逊',
                shopShort: 'yama',
                id: 'gangsate',
                status: false,
                idShort: 'a'
            }, {
                account: 'a',
                shop: '速卖通',
                shopShort: 'sumai',
                id: 'gangsate',
                status: true,
                idShort: 'a'
            }],
        }
    },
    methods:{
        filerate(val){
            console.log(val);
            this.curStatus = val;
        },
        search(val){
                console.log(val);
        },
    },
    computed:{
        comtableData(){
            if(this.curStatus===''){
                return this.tableData
            }else{
                return this.tableData.filter(row=>row.status===this.curStatus)
            }
        }
    },
    components: {
        headerPanel,
        footPanel,
        tableShow
    },
}

</script>


