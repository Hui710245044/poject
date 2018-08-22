<template>
  <div>
    <el-button type="primary" @click.native="addData">添加</el-button>
    <el-table :data="tableData" border style="width: 100%">
          <el-table-column prop="shop" sortable label="平台店铺">
          </el-table-column>

          <el-table-column prop="shopShort" sortable label="店铺简称">
          </el-table-column>

          <el-table-column prop="id" sortable label="平台账号">
          </el-table-column>

          <el-table-column prop="idShort" sortable label="账号简称">
          </el-table-column>

          <el-table-column label="数据同步">
            <template slot-scope="scope">
              <img src="../assets/downloadOff.png" alt="">
              <img src="../assets/syncOff.png" alt="">
              <img src="../assets/targetOff.png" alt="">
            </template>
        </el-table-column>

        <el-table-column label="系统状态">
            <template slot-scope="scope">
                  <el-switch v-model="tableData[scope.$index].status" active-text="已启用" inactive-text="已禁用">
                  </el-switch>
                </template>
        </el-table-column>

        <el-table-column fixed="right" label="操作">
            <template slot-scope="scope">
                  <el-button @click.native="lookUp(scope.row)" type="text">查看</el-button>
                  <el-button type="text" @click.native="handleEdit(scope.row)">编辑</el-button>
                  <el-button @click.native.prevent="deleteRow(scope.$index, tableData)" type="text" size="small">
                    移除
                  </el-button>
                </template>
        </el-table-column>
    </el-table>
        <!-- 查看     -->
        <el-dialog title="查看内容" :visible.sync="lookDialog">
            <el-form :model="addEditform">
                <el-form-item label="平台店铺" :label-width="formLabelWidth">
                    <el-input v-model="addEditform.shop" auto-complete="off" disabled></el-input>
                </el-form-item>
                <el-form-item label="店铺简称" :label-width="formLabelWidth">
                    <el-input v-model="addEditform.shopShort" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="平台账号" :label-width="formLabelWidth">
                    <el-input v-model="addEditform.id" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="账号简称" :label-width="formLabelWidth">
                    <el-input v-model="addEditform.id" auto-complete="off"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="lookDialog = false">取 消</el-button>
                <el-button type="primary" @click="update">确 定</el-button>
            </div>
        </el-dialog>
        <!-- 添加 -->
        <el-dialog title="添加店铺" :visible.sync="addDialog">
            <el-form :model="addEditform">
                <el-form-item label="* 平台账号:" :label-width="formLabelWidth">
                    <el-select v-model="addEditform.account" placeholder="请选择">
                        <el-option label="gangsate" value="shanghai"></el-option>
                        <el-option label="gangsate" value="beijing"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="* 平台店铺:" :label-width="formLabelWidth">
                    <el-input v-model="addEditform.shop" auto-complete="off" style="width:215px"></el-input>
                </el-form-item>
                <el-form-item label="* 店铺简称:" :label-width="formLabelWidth">
                    <el-input v-model="addEditform.shopShort" auto-complete="off" style="width:215px">></el-input>
                </el-form-item>
                <el-form-item label="商户ID:" :label-width="formLabelWidth">
                    <el-input v-model="addEditform.id" auto-complete="off" style="width:215px">></el-input>
                </el-form-item>
                <el-form-item label="抓取Listing数据:" :label-width="formLabelWidth">
                    <el-select v-model="addEditform.account" placeholder="请选择">
                        <el-option label="未启用" value="shanghai"></el-option>
                        <el-option label="1小时" value="beijing"></el-option>
                        <el-option label="2分钟" value="beijing"></el-option>
                        <el-option label="3分钟" value="beijing"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="抓取Joom订单功能:" :label-width="formLabelWidth">
                    <el-select v-model="addEditform.account" placeholder="请选择">
                        <el-option label="未启用" value="shanghai"></el-option>
                        <el-option label="15分钟" value="beijing"></el-option>
                        <el-option label="30分钟" value="beijing"></el-option>
                        <el-option label="60分钟" value="beijing"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="同步发货状态到Joom功能:" :label-width="formLabelWidth">
                    <el-select v-model="addEditform.label" placeholder="请选择">
                        <el-option label="未启用" value="shanghai"></el-option>
                        <el-option label="15分钟" value="beijing"></el-option>
                        <el-option label="30分钟" value="beijing"></el-option>
                        <el-option label="60分钟" value="beijing"></el-option>
                    </el-select>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="addDialog = false">取 消</el-button>
                <el-button type="primary" @click="submit">确 定</el-button>
            </div>
        </el-dialog>
        <!-- 编辑 -->
        <el-dialog title="编辑内容" :visible.sync="Edit">
            <el-form :model="addEditform">
                <el-form-item label="平台店铺" :label-width="formLabelWidth" >
                    <el-input v-model="addEditform.shop" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="店铺简称" :label-width="formLabelWidth">
                    <el-input v-model="addEditform.shopShort" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="平台账号" :label-width="formLabelWidth">
                    <el-input v-model="addEditform.id" auto-complete="off"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="Edit = false">取 消</el-button>
                <el-button type="primary" @click="submit">确 定</el-button>
            </div>
        </el-dialog>

</div>
</template>
<script>
//    import event from '../enent.js'
    export default {
        methods: {
            handleClick(row) {
                console.log(row);
            },
            deleteRow(index, rows) {
                rows.splice(index, 1);
            },
            submit() {
                let find = this.tableData.find(row => row.shop === this.addEditform.shop);
                if (find) return this.$message({
                    type: "warning",
                    message: "平台账号重复,请修改"
                });
                this.tableData.unshift(this.addEditform);
                this.addDialog = false;
            },
            addData() {
                this.addDialog = true;
                this.addEditform = {
                    shop: '',
                    shopShort: '',
                    id: '',
                    idShort: '',
                }
            },
            handleEdit(row) {
                this.lookDialog = true;
                this.addEditform = row;
            },
            update() {
                let find = this.tableData.find(
                    row => row.shop === this.addEditform.shop);
                Object.assign(find, this.addEditform);
                this.lookDialog = false;
            },
            lookUp(row) {
                this.lookDialog = true;
                this.addEditform = row;
            }
        },
        props:{
            tableData:{
                type:Array,
                required:true
            }
        },
        data() {
            return {
                addDialog: false,
                lookDialog: false,
                Edit: false,
                addEditform: {
                    account: '',
                    shop: '',
                    shopShort: '',
                    id: '',
                    status: true,
                    idShort: '',
                    label:''
                },
                formLabelWidth: '100px',

            };
        }
    };
</script>
<style>
  
    img {
        width: 20px;
        height: 20px;
    }
</style>

        <!--编辑弹层-->
        <!--<el-dialog title="编辑XXX信息" :visible.sync="editDialog">-->
            <!--<el-form :model="addEditform">-->
                <!--<el-form-item label="来源:" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="addEditform.from_tag" auto-complete="off" class="input-size"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="物流商:" :label-width="formLabelWidth">-->
                    <!--<el-select v-model="addEditform.channel" placeholder="请选择" class="input-size">-->
                        <!--&lt;!&ndash;<el-option&ndash;&gt;-->
                                <!--&lt;!&ndash;v-for="channel in channels"&ndash;&gt;-->
                                <!--&lt;!&ndash;:key="channel.node"&ndash;&gt;-->
                                <!--&lt;!&ndash;:label="channel.channel"&ndash;&gt;-->
                                <!--&lt;!&ndash;:value="channel.node">&ndash;&gt;-->
                        <!--&lt;!&ndash;</el-option>&ndash;&gt;-->
                    <!--</el-select>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="妥投状态:" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="addEditform.is_succ" auto-complete="off" class="input-size"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="妥投截止时间:" :label-width="formLabelWidth">-->
                    <!--<el-date-picker-->
                            <!--v-model="addEditform.etime"-->
                            <!--type="date"-->
                            <!--placeholder="选择日期">-->
                    <!--</el-date-picker>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="跟踪号:" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="addEditform.track" class="input-size"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="运输方式:" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="addEditform.transport" class="input-size"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="上网状态:" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="addEditform.is_online" class="input-size"></el-input>-->
                <!--</el-form-item>-->
            <!--</el-form>-->
            <!--<div slot="footer" class="dialog-footer">-->
                <!--<el-button @click="editDialog = false">取 消</el-button>-->
                <!--<el-button type="primary" @click="update">确 定</el-button>-->
            <!--</div>-->
        <!--</el-dialog>-->
        <!--轨迹-->
        <!--<el-dialog title="查看跟踪号:轨迹" :visible.sync="dialogTableVisible">-->
            <!--<el-table :data="addEditform">-->
                <!--<el-table-column property="date" label="时间" width="150"></el-table-column>-->
                <!--<el-table-column property="content" label="内容" width="200"></el-table-column>-->
            <!--</el-table>-->
        <!--</el-dialog>-->