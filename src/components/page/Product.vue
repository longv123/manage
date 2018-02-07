<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i> 产品管理</el-breadcrumb-item>
                <el-breadcrumb-item></el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="handle-box">
            <el-form :inline="true" :model="searchInfo">
                <el-form-item label=产品名：>
                    <el-input v-model="searchInfo.searchName" placeholder="请输入查询的产品名称"></el-input>
                </el-form-item>
                <el-form-item label=产品编号：>
                    <el-input v-model="searchInfo.searchId" placeholder="请输入查询的产品编号"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="search" icon="search">查询</el-button>
                </el-form-item>
            </el-form>
        </div>
        <div class="table-box">
            <table border="1" bordercolor='#D2D2D2' class='product_table' cellspacing="0" cellpadding="0" frame="void">
                <tr style='background-color: #F5F6FA;' class='product_table_header'>
                    <th width='20%'>产品名</th>
                    <th width='20%'>产品编号</th>
                    <th width='10%'>售卖价</th>
                    <th width='20%'>所属分类</th>
                    <th width='20%'>当前库存/件</th>
                    <th width='10%'>操作</th>
                </tr>
                <tr v-for='(item,index) in productDetail' >
                    <td colspan='6'>
                        <table border='1' bordercolor='#D2D2D2' class='table_child' cellspacing="0" cellpadding="0" frame="void">
                            <tr class='table_child_header'>
                                <td width='20%'>{{item.name}}</td>
                                <td width='20%'>{{item.id}}</td>
                                <td width='10%'>{{item.price}}</td>
                                <td width='20%'>{{item.types}}</td>
                                <td @click = '' width='20%' class='select_not'>{{item.stock}}</td>
                                <td @click='isHide(index)' width='10%' class='select_not'>{{item.stockDetail}}</td>
                            </tr>
                            <tr class='tr_frame' v-show='frameHide & activeIndex == index'>
                                <td colspan='6'>
                                    <table class='table_frame' border="1" bordercolor='#D2D2D2' cellspacing="0" cellpadding="0">
                                        <tr style='background-color: #F5F6FA'>
                                            <th>规格值</th>
                                            <th>商品sku</th>
                                            <th>当前库存/件</th>
                                        </tr>
                                        <tr v-for='(item,index) in colorData'>
                                            <td>{{item.goodsColor}}</td>
                                            <td>{{item.goodsSku}}</td>
                                            <td class='select_not'>{{item.goodsStock}}</td>
                                        </tr>
                                    </table>
                                </td>
                            </tr>
                        </table>
                    </td>
                </tr>
            </table>
        </div>
        <!--<el-table :data="tableData2" style="width: 100%" :border=true>
            <el-table-column type="expand" v-show='isExpand'>
                <template slot-scope="props">
                    <el-table ref="singleTable" :data="tableData" style="width:50%;margin: 0 25%" :border=true >
                        <el-table-column property="goodsColor" label="规格值" width="120" align='center'></el-table-column>
                        <el-table-column property="goodsSku" label="商品sku" width="250" align='center'></el-table-column>
                        <el-table-column property="goodsStock" label="当前库存/件" align='center' style='color:red'></el-table-column>
                    </el-table>
                </template>
            </el-table-column>
            <el-table-column label="产品名" prop="name" align='center' ></el-table-column>
            <el-table-column label="产品编号" prop="id" align='center' ></el-table-column>
            <el-table-column label="售卖价" prop="price" align='center' ></el-table-column>
            <el-table-column label="所属分类" prop="types" align='center'></el-table-column>
            <el-table-column label="当前库存/件" prop="stock" align='center'>
                <template slot-scope="scope">
                <el-button @click="handleClick(scope.row.index)" type="text" size="medium">{{scope.row.stock}}</el-button>
            </template>
            </el-table-column>
            <el-table-column label="操作" prop="stockDetail" align='center'>
                <template slot-scope="scope">
                    <el-button @click="handleClick(scope.row)" type="text" size="medium">商品（4件）</el-button>
                </template>
            </el-table-column>
        </el-table>-->
    </div>
</template>

<script>
    export default {
        data() {
            return {
                is_search: false,
                isExpand: false,
                frameHide:false,
                activeIndex: '',
                searchInfo: {
                    searchId: '',
                    searchName: ''
                },
                colorData: [{
                    goodsColor: '中国红',
                    goodsSku: 'RYDXX0308-1',
                    goodsStock: '122'
                }, {
                    goodsColor: '传奇黑',
                    goodsSku: 'BYDXX0308-1',
                    goodsStock: '110'
                }, {
                    goodsColor: '樱花粉',
                    goodsSku: 'PYDXX0308-1',
                    goodsStock: '122'
                }, {
                    goodsColor: '经典白',
                    goodsSku: 'WYDXX0308-1',
                    goodsStock: '110'
                }],
                productDetail: [{
                    name: '逸豆（尊贵版）',
                    id: 'ET-YiDou 0308-1',
                    price: '¥ 2699.00',
                    types: '硬件设备',
                    stock: '232',
                    stockDetail: '商品（4件）'
                }, {
                    name: '逸豆',
                    id: 'ET-YiDou 0308-0',
                    price: '¥ 1799.00',
                    types: '硬件设备',
                    stock: '431',
                    stockDetail: '商品（4件）'
                }]
            }
        },
        created() {
        },
        computed: {},
        methods: {
            handleCurrentChange(val) {
                this.cur_page = val;
            },
            search() {
                this.is_search = true;
            },
            formatter(row, column) {
                return row.address;
            },
            filterTag(value, row) {
                return row.tag === value;
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            },
            handleClick(row) {
                console.log(row);
            },
            isHide(index){
                if (this.activeIndex ==index) {
                    this.frameHide = !this.frameHide
                }
                else {
                    this.activeIndex = index
                    this.frameHide = true
                }
            }
        },
        watch:{
            activeIndex:function (m,n) {
            }
        }
    }
</script>

<style scoped>
    /*搜索模块*/
    .handle-box {
        margin-bottom: 20px;
    }
    .table-box{
        border: 1px solid #D2D2D2;
    }
    /*列表模块*/
    .table_child tr:hover,.table_frame tr:hover{
        background-color: #F5F6FA;
    }
    .product_table,.table_frame,.table_child {
        border-collapse: collapse;
        width: 100%;
        font-size: 15px;
        color: rgb(0, 1, 32);
    }
    .select_not{
        color: #20A0FF;
        cursor: pointer;
        text-decoration: underline;
        moz-user-select: -moz-none;
        -moz-user-select: none;
        -o-user-select:none;
        -khtml-user-select:none;
        -webkit-user-select:none;
        -ms-user-select:none;
        user-select:none;
    }
    /*父表格*/
    .product_table th{
        /*border: 1px solid #D2D2D2;*/
        height: 50px;
    }
    .product_table .product_table_header{
        /*border: 1px solid #D2D2D2;*/
    }
    /*子表格*/
    /*.table_child{
        border-top: 1px solid #fff;
    }*/
    .table_child_header td{
        border: 1px solid #D2D2D2;
    }
    .table_child td {
        height: 40px;
        line-height: 40px;
        text-align: center;
    }
    /*表格弹框*/
    .table_frame{
        width: 500px;
        margin: 10px auto;
        text-align: center;
        font-size: 14px;
    }
    .table_frame td,.table_frame th{
        height: 30px;
    }
    .tr_frame{
        background: rgba(245,246,250,0.2)!important;
        z-index: 5;
    }
</style>
