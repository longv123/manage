<template>
    <div class="shipments">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i> 产品管理</el-breadcrumb-item>
                <el-breadcrumb-item>商品明细</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="handle-box">
            <el-form :inline="true">
                <div class='box-top'>
                    <label>产品：</label><input value="烦烦烦烦烦烦" readonly/>
                    <label>产品：</label><input value="cccccc" readonly/>
                    <label>产品：</label><input value="不过不够不够" readonly/>
                </div>
                <el-form-item label="商品SN：" la>
                    <el-input v-model="searchData.sku" placeholder="输入查询商品SN"></el-input>
                </el-form-item>
                <el-form-item label="商品sku：">
                    <el-input v-model="searchData.sku" placeholder="输入查询商品sku"></el-input>
                </el-form-item>
                <el-form-item label="存放仓库：">
                    <el-select @change="searchData.warehouseId=''" v-model="wareAreaId" placeholder="请选择" class="handle-select">
                        <el-option v-for="(item,index) in warehouse" :key="item.value" :label="item.name" :value="index"></el-option>
                    </el-select>
                    <el-select v-model="searchData.warehouseId" placeholder="请选择" class="handle-select2">
                        <el-option v-for="(item,index) in warehouse[wareAreaId].childNode" :key="item.value" :label="item.name" :value="item.warehouseId"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="入库时间：">
                    <el-date-picker
                        v-model="pickTime"
                        type="datetimerange"
                        range-separator="至"
                        start-placeholder="开始日期"
                        end-placeholder="结束日期"
                        placeholder="选择拣货时间">
                    </el-date-picker>
                </el-form-item>
                <el-button type="primary" icon="search">搜索</el-button>
            </el-form>
        </div>
        <!-- 列表 -->
        <div class="list">
            <table width="100%" class="ship_table">
                <tr style="background-color: #F5F6FA">
                    <th style="width:40px">序号</th>
                    <th>关联单号</th>
                    <th>商品</th>
                    <th>商品sku</th>
                    <th>商品数量</th>
                    <th>发货仓库</th>
                    <th>变动原因</th>
                    <th>收货人信息</th>
                    <th>拣货人</th>
                    <th style="width:90px">拣货时间</th>
                    <th>操作</th>
                </tr>
                <tr style="height:69px"  v-for="(item,index) in listData">
                    <td>{{index+1}}</td>
                    <td>{{item.orderId}}</td>
                    <td style="text-align:left">产品名：{{item.productName}}<br />规格：{{item.productNormName}}</td>
                    <td>{{item.sku}}</td>
                    <td style="color:#5200FF;text-decoration:underline;cursor:pointer" @click="getProductGoods(item.id)">111</td>
                    <td style="text-align:left">仓位：{{item.warehouse }}<br />地址：{{item.warehouseAddress }}</td>
                    <td>{{item.reason}}</td>
                    <td style="text-align:left">{{item.name}}，{{item.phone}}，{{item.address}}</td>
                    <td style="text-align:left">姓名：{{item.operator.name}}<br />电话：{{item.operator.phone}}</td>
                    <td>{{item.updateTime }}</td>
                    <td @click="sendGoods(item.id)"><a style="color:blue;text-decoration:underline;cursor:pointer">发货</a></td>
                </tr>
            </table>
        </div>
        <!-- 商品清单弹框 -->
        <div class="box" v-show="productListBoxShow">
            <div class="list_bombBox">
                <div class="shipBomb_title" style="height:50px;line-height:50px;padding:0 5%;background-color: #E9EAEA">
                    <span>商品清单</span>
                    <span style="float:right;cursor:pointer" @click="productListBoxShow=false">X</span>
                </div>
                <div class="ship_con">
                    <table class="ship_table" style="margin:10px auto 30px;width:90%">
                        <tr style="background-color: #F5F6FA">
                            <th style="width:50px">序号</th>
                            <th>商品SN码</th>
                        </tr>
                        <tr style="height:40px" v-for='(item,index) in goodsList'>
                            <td>{{index+1}}</td>
                            <td>{{item}}</td>
                        </tr>
                    </table>
                </div>
            </div>
        </div>

        <!-- 发货弹框 -->
        <div class="box" v-show="pendBombBoxShow">
            <div class="pend_bombBox" style="width:550px;left:50%;margin-left:-200px">
                <div class="shipBomb_title" style="height:50px;line-height:50px;padding:0 5%;background-color: #E9EAEA">
                    <span>商品清单</span>
                    <span style="float:right;cursor:pointer" @click="pendBombBoxShow=false">X</span>
                </div>
                <div class="ship_con" style="padding:10px 5% 20px;max-height:400px">
                    <div class="proName"><label for="">产品名：</label>一代翻译机</div>
                    <div class="proName"><label for="">规格：</label>111</div>
                    <div class="proName"><label for="">收件人信息：</label>张全蛋，182345671234，北京市海淀区上地七街</div>
                    <table class="ship_table" width="100%" style="margin: 20px 0 20px">
                        <tr style="background-color: #F5F6FA">
                            <th style="width:50px">序号</th>
                            <th>商品SN码</th>
                        </tr>
                        <tr style="height:40px" v-for='(item,index) in goodsList'>
                            <td>{{index+1}}</td>
                            <td>{{item}}</td>
                        </tr>
                    </table>
                    <div class="sendType"><label for="">快递类型<a style="color:red">*</a>：</label><el-input class="handle-select1 mr10" v-model="expressType"></el-input></div>
                    <div class="pending_warn">
                        <p v-show="warn.warn1">快递类型不能为空</p>
                        <p v-show="warn.warn2">快递类型不能超过20个字哦</p>
                    </div>
                    <div class="sendNum" style="margin:10px 0;"><label for="">快递单号<a style="color:red">*</a>：</label><el-input class="handle-select1 mr10" v-model="expressCode"></el-input></div>
                    <div class="pending_warn">
                        <p v-show="warn.warn3">快递单号不能为空</p>
                        <p v-show="warn.warn4">快递单号不能超过15个字哦</p>
                    </div>
                    <div class="pendBox_btn" style="margin:10px 0;">
                        <el-button style="background:#ccc;color:#fff;width:88px;margin-left:170px" @click="pendBombBoxShow=false">取消</el-button>
                        <el-button type="primary" @click="deliverGood">确认发货</el-button>
                    </div>
                </div>
            </div>
        </div>

        <div class="pagination">
            <el-pagination
                @current-change ="handleCurrentChange"
                layout="prev, pager, next"
                :total="total"
                v-show="total != 0">
            </el-pagination>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                //获取的下拉菜单数据
                warehouse: [
                    {
                        warehouseId: "",
                        name: "全部",
                        childNode: []
                    }
                ], //仓
                wareAreaId: 0, //区
                // sendTime:'',//发货时间
                pickTime: "", //拣货时间
                // 搜索数据
                searchData: {
                    orderId: "", //关联单号
                    productName: "", //商品
                    productNormName: "", //规格
                    sku: "", //商品sku
                    warehouseId: "", //仓库分区ID
                    reason: "", //变动原因
                    name: "", //收货人
                    operatorName: "", //拣货人
                    sendStartDate: "", //拣货时间(开始)
                    sendendDate: "", //拣货时间(结束)
                    pageStart: 1, //页码
                    pageSize: 10 //每页数量
                },
                // 列表数据
                listData: [],
                total: 0, //列表数据总数
                productListBoxShow: false, //商品清单弹框
                goodsList:[],//商品清单
                pendBombBoxShow: false ,//发货弹框
                deliverGoodId: '',//发货id
                expressType:'',//快递类型
                expressCode:'',//快递单号
                warn:{//发货单号、发货类型校验
                    warn1:false,//类型空
                    warn2:false,//类型大于20
                    warn3:false,//单号空
                    warn4:false,//单号大于15
                }
            };
        },
//   created() {
//     this.getData();
//   },
        beforeRouteEnter(to, from, next) {
            next(vm => {
                vm.getData(); //获取列表数据
                vm.getWarehouse(); //获取仓库
            });
        },
        watch: {
            //   拣货时间格式化
            pickTime(curVal, oldVal) {
                if (curVal[0] == "" || curVal[0] == null) {
                    this.searchData.sendStartDate = "";
                    this.searchData.sendendDate = "";
                } else {
                    this.searchData.sendStartDate = this.time(curVal[0]);
                    this.searchData.sendendDate = this.time(curVal[1]);
                }
                //   console.log(this.sendStartDate+'=='+this.sendendDate)
            },
            // 发货弹框
            pendBombBoxShow(curVal,oldVal){
                if(curVal == false){
                    this.expressType='',//快递类型
                        this.expressCode='',//快递单号
                        this.warn={//发货单号、发货类型校验
                            warn1:false,//类型空
                            warn2:false,//类型大于20
                            warn3:false,//单号空
                            warn4:false,//单号大于15
                        }
                }
            }
        },

        methods: {
            handleCurrentChange(val) {
                this.searchData.pageStart = val;
                this.getData();
            },
            // 時間戳改為yyyy-MM-dd HH:mm:ss
            time(input) {
                var d = new Date(input);
                var year = d.getFullYear();
                var month = d.getMonth() + 1;
                month = month < 10 ? "0" + month : "" + month;
                var day = d.getDate() < 10 ? "0" + d.getDate() : "" + d.getDate();
                var hour = d.getHours() < 10 ? "0" + d.getHours() : "" + d.getHours();
                var minutes =
                    d.getMinutes() < 10 ? "0" + d.getMinutes() : "" + d.getMinutes();
                var seconds =
                    d.getSeconds() < 10 ? "0" + d.getSeconds() : "" + d.getSeconds();
                return (
                    year +
                    "-" +
                    month +
                    "-" +
                    day +
                    " " +
                    hour +
                    ":" +
                    minutes +
                    ":" +
                    seconds
                );
            },
            // 获取仓库
            getWarehouse() {
                this.$indexServer.getWarehouse().then(res => {
                    if (res.data.code == 200) {
                        this.warehouse = res.data.data.warehouse;
                    }
                });
            },
            // 获取列表数据
            getData() {
                this.$indexServer.waitDeliverGoodsList(this.searchData).then(res => {
                    if (res.data.code == 200) {
                        this.listData = res.data.data.sendGoodsRecord;
                        this.total = res.data.total
                    }
                });
            },
            // 查看明细
            getProductGoods(id) {
                this.$indexServer.getproductGoodsByStockId(id).then(res => {
                    if (res.data.code == 200) {
                        this.goodsList = res.data.data.productGoods;
                        this.productListBoxShow = true;
                    }
                });
            },
            // 点击发货按钮
            sendGoods(id){
                this.$indexServer.getproductGoodsByStockId(id).then(res => {
                    if (res.data.code == 200) {
                        this.goodsList = res.data.data.productGoods;
                        this.pendBombBoxShow = true
                        this.deliverGoodId=id
                    }
                });
            },
            // 发货前校验
            checkExpress(){
                this.warn ={
                    warn1:false,
                    warn2:false,
                    warn3:false,
                    warn4:false,
                }
                if(this.expressType.length<=0){//类型空
                    this.warn.warn1 = true
                    return false
                }else if(this.expressType.length>20){//类型大于20
                    this.warn.warn2 = true
                    return false
                }else if(this.expressCode.length<=0){//单号空
                    this.warn.warn3 = true
                    return false
                }else if(this.expressCode.length>15){//单号大于15
                    this.warn.warn4 = true
                    return false
                }else{
                    return true
                }
            },
            // 确认发货
            deliverGood(){
                if(this.checkExpress()){
                    this.$indexServer.deliverGood(this.deliverGoodId,this.expressType,this.expressCode).then(res => {
                        if (res.data.code == 200) {
                            this.pendBombBoxShow = false
                            this.$message({
                                message: '发货成功',
                                type: 'success'
                            });
                            this.getData()
                        }
                    });
                }

            },
        }
    };
</script>

<style scoped>
    .box-top{
        font-size: 14px;
        color: #48576a;
        margin-bottom: 22px;
    }
    .box-top label{
        padding: 11px 12px 11px 0;
    }
    .box-top input{
        padding: 3px 10px;
        height: 30px;
        line-height: 30px;
        min-width: 200px;
        margin-right: 20px;
        border: none;
        color: #1f2d3d;
    }
    .shipments {
        min-width: 1024px;
    }
    .handle-box {
        margin-bottom: 20px;
    }
    .handle-select {
        width: 120px;
    }
    .handle-select1 {
        width: 350px;
    }
    .handle-select2 {
        width: 160px;
    }
    .handle-input {
        width: 300px;
        display: inline-block;
    }
    .ship_table {
        table-layout: fixed;
        empty-cells: show;
        border-collapse: collapse;
        margin: 0 auto;
    }

    .ship_table td,
    .ship_table th {
        border: 1px solid rgb(210, 210, 210);
        padding: 0 5px 0;
        text-align: center;
        vertical-align: middle;
        font-size: 12px;
        color: rgb(0, 1, 32);
    }

    .ship_table th {
        background-repeat: repeat-x;
        height: 50px;
        font-size: 14px;
    }

    a {
        color: rgb(0, 1, 32);
    }
    .box {
        position: fixed;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.6);
        z-index: 9999;
    }
    .list_bombBox,
    .pend_bombBox {
        border: 1px solid #e9eaea;
        width: 30%;
        background-color: #fff;
        position: absolute;
        top: 20%;
        left: 40%;
    }
    .ship_con div {
        color: #1f2d3d;
        word-wrap: break-word;
    }
    .ship_con label {
        font-size: 14px;
        color: #48576a;
    }
    .list_bombBox .ship_con,
    .pend_bombBox .ship_con {
        overflow-x: hidden;
        overflow-y: auto;
    }
    .ship_con .pending_warn p{
        background: red;
       /* background: url('./mark.png') no-repeat left;*/
        padding-left: 20px;
        color: #FB654E;
        font-size: 14px;
        margin-left: 76px;
    }
    /* .list_bombBox .ship_con::-webkit-scrollbar,.pend_bombBox .ship_con::-webkit-scrollbar{
        width: 0.06rem;
        height: 0.1rem;
    }
    .list_bombBox .ship_con::-webkit-scrollbar-track,.pend_bombBox .ship_con::-webkit-scrollbar-track{
        -webkit-box-shadow: inset 0 0 0.06rem rgba(0, 0, 0, 0.3);
        border-radius: 8px;
        background-color: #f5f5f5;
    }
    .list_bombBox .ship_con::-webkit-scrollbar-thumb, .pend_bombBox .ship_con::-webkit-scrollbar-thumbr{
        border-radius: 8px;
        -webkit-box-shadow: inset 0 0 0.06rem rgba(0, 0, 0, 0.3);
        background-color: #999999;
    } */
</style>
