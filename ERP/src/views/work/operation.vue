<template>
    <section :style="{height: $store.state.home.modelContentHeight + 'px'}">
        <!-- 顶部 -->
        <header class="operation_top">
            <p class='operation_title'>操作日志</p>
        </header>
        <section class="operation_conent" >
            <div class="operation_box" :style="{height: $store.state.home.modelContentHeight-23 + 'px'}">
                <div class='box_top'>
                     <el-input
                        size="small"
                        placeholder="请输入内容"
                        clearable
                        prefix-icon="el-icon-search"
                        v-model="input"
                        style="width:378px">
                    </el-input>
                    <el-button type="primary" size='small' style="margin-left:10px" @click="find">搜索</el-button>
                </div>
                <el-table
                    :data='date'
                    :height='$store.state.home.modelContentHeight-78'>
                    <el-table-column
                        prop=""
                        width='30'>
                    </el-table-column>
                    <el-table-column
                        prop="userName"
                        label='操作人'
                        width='150'>
                    </el-table-column>
                    <el-table-column
                        prop="objectId"
                        label='操作对象'
                        width='150'>
                    </el-table-column>
                    <el-table-column
                        label='ID'
                        width='200'>
                        <template slot-scope="scope">
                            <span style="color:#3f9ffe">{{scope.row.objectId}}</span>
                        </template>
                    </el-table-column>
                    <el-table-column
                        prop="operateType"
                        label='操作说明'
                        width='200'>
                    </el-table-column>
                    <el-table-column
                        prop="afterData"
                        label='操作状态'>
                    </el-table-column>
                    <el-table-column
                        label='操作时间'
                        width='250'>
                        <template slot-scope="scope">
                            <span style="color:#3f9ffe">{{scope.row.created | time_m}}</span>
                        </template>
                    </el-table-column>
                    <el-table-column
                        prop="ipAddress"
                        label='IP'
                        width='200'>
                    </el-table-column>
                </el-table>
            </div>
        </section>
        <footer class="operation_footer">
            <div class="block">
                <el-pagination
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page="currentPage"
                    :page-sizes="[10, 30, 50, 100]"
                    :page-size="10"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="totalPage">
                </el-pagination>

            </div>
        </footer>
    </section>
</template>
<script>
import api from 'api/work'

export default {
    name:'operation',
    data() {
        return {
            date:[],

            currentPage: 2,
            totalPage: 1,

            input: '',

            page:[]
        }
    },
    methods: {
        handleSizeChange(val) {
            this.page.pageSize = val
        },
        handleCurrentChange(val) {
            this.page.pageNo = val
        },
        get() {
            api.getactionloglist().then((response)=>{

                // console.log(response.data.list)
                this.date = response.data.list

            }).catch((error)=>{

                console.log(error)

            })
        },
        find() {
            let obj = {
                userName: this.input
            }

            api.getactionloglist(obj).then((response)=>{

                // console.log(response.data.list)
                this.date = response.data.list

            }).catch((error)=>{

                console.log(error)

            })
        }
    },
    created(){
        // console.log(this.form)
        this.get()

    },
    activated() {
        this.get()
    }
}
</script>
<style scoped>
/* 顶部 */
.operation_top{
    display: flex;
    justify-content: space-between;
    height: 46px;
    padding: 10px;
    border-bottom: 1px solid #e5e8e8
}
.operation_title{
    height: 25px;
    line-height: 25px;
    margin-left: 20px;
    font-size: 15px;
    color: #5e6161
}
.operation_top .el-input{
    width: 40%;
}
/* 内容 */
.operation_conent{
    padding: 10px;
    background: #f5f5f5;
}
.operation_box{
    border: 1px solid #e6e9eb;
    color: #5e6161;
    background: white
}
.box_top{
    display: flex;
    background:white;
    padding: 16px
}
.box_top .el-input{
    width: 35%;
}

/* 底部 */
.operation_footer{
    border-top:1px solid #e5e8e8;
    position: relative;
    padding: 11px 30px;
    /* margin-top: -23px; */
    background: white;
}
.operation_footer .block{
    position: absolute;
    right: 30px;
    top:0px
}
</style>
