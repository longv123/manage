<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-date"></i> 库存管理</el-breadcrumb-item>
                <el-breadcrumb-item>商品出库</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="form-box">
            <el-form ref="form" :model="form" label-width="100px" :inline='true' :rules="rules">
                <el-form-item label="产品：" required>
                    <el-select @change='clearId()' v-model="cateTreeId" placeholder="请选择" style='width: 180px'>
                        <el-option v-for="(item,index) in cateTree" :key="item.value" :label="item.name" :value="index"></el-option>
                    </el-select>
                    <el-select @change='getProductColor(productId)' v-model="productId" placeholder="请选择产品" style='width: 220px'>
                        <el-option v-for="(item,index) in cateTree[cateTreeId].childNode" :key="item.value" :label="item.name" :value="item.id"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="规格：" label-width="64px">
                    <el-select @change='getProductSku()' v-model="normAttrId" placeholder="请选择商品规格" style='width: 242px'>
                        <el-option v-for="(item,index) in productColor" :key="item.value" :label="item.name" :value="item.productNormAttrId"></el-option>
                    </el-select>
                </el-form-item>
                    <div class='box-top'>
                        <label>商品SKU：</label><input v-model='productSku' readonly/>
                    </div>
                <el-form-item label="存放仓库：">
                    <el-select @change="form.warehouseId=''" v-model="wareAreaId" placeholder="请选择" class="handle-select" style='width: 150px'>
                        <el-option v-for="(item,index) in warehouse" :key="item.value" :label="item.name" :value="index"></el-option>
                    </el-select>
                    <el-select v-model="form.warehouseId" placeholder="请选择" class="handle-select2" style='width: 180px'>
                        <el-option v-for="(item,index) in warehouse[wareAreaId].childNode" :key="item.value" :label="item.name" :value="item.warehouseId"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="出库原因：" label-width='90px'>
                    <el-select v-model="form.reason" placeholder="请选择出库原因" style='width: 286px'>
                        <el-option key="0" label="APP购买订单发货" value="0"></el-option>
                        <el-option key="1" label="【逸豆带你看世界】服务号发货" value="1"></el-option>
                        <el-option key="2" label="逸途旅行easyto公众号发货" value="2"></el-option>
                        <el-option key="3" label="APP金卡兑换订单发货" value="3"></el-option>
                        <el-option key="4" label="赠送" value="4"></el-option>
                        <el-option key="5" label="换货" value="5"></el-option>
                        <el-option key="6" label="返厂保修" value="6"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="关系人信息" class='related' style='display: block'></el-form-item>
                <el-form-item label="姓名：" prop="name">
                    <el-input v-model="form.name" style='width: 305px'></el-input>
                </el-form-item>
                <el-form-item label="电话：" prop="phone" label-width='60px'>
                    <el-input v-model="form.phone" style='width: 345px'></el-input>
                </el-form-item>
                <el-form-item label="关联单号：" prop="orderId">
                    <el-input v-model="form.orderId" style='width:730px'></el-input>
                </el-form-item>
                <el-form-item label="地址：" prop="address">
                    <el-input v-model="form.address" style='width: 730px'></el-input>
                </el-form-item>
                <el-form-item label="备注：">
                    <el-input type="textarea" v-model="form.remark" style='width: 730px'></el-input>
                </el-form-item>
                <div class='box_scan'>
                    <label>商品SN录入：</label><input value="默认扫码录入，请连接扫码枪，进行扫码录入。" readonly/>
                </div>
                <el-form-item style='margin-left: 283px'>
                    <el-button style='width: 120px'>取消</el-button>
                    <el-button type="primary" @click="onSubmit('form')" style='width: 120px'>完成出库</el-button>
                </el-form-item>
            </el-form>
        </div>

    </div>
</template>

<script>
    import ElFormItem from "../../../node_modules/element-ui/packages/form/src/form-item";
    export default {
        components: {ElFormItem},
        data: function () {
            return {
                form: {
                    productGoodId: '', //货品id
                    warehouseId: '',   //仓库分区id
                    reason: '',        //出库原因
                    orderId: '',       //关联单号
                    name: '',          //关系人
                    phone: '',         //电话
                    address: '',       //地址
                    remark: '',        //备注
                    sn: ''             //商品SN码(数组)
                },
                cateTree: [
                    {
                        id: "",
                        name: "",
                        childNode: []
                    }
                ],                      //产品种类
                cateTreeId: 0,         //产品一级id
                productColor:'',       //产品规格（颜色）
                productId:'',          //产品二级id
                normAttrId:'',         //属性id(颜色)
                productSku: '请先选择产品',            //sku
                warehouse: [
                    {
                        warehouseId: "",
                        name: "",
                        childNode: []
                    }
                ],                     //获取的仓库下拉菜单数据
                wareAreaId:0,         //仓库分区ID
                rules: {
                    remark: [
                        {required: true, max: 2, message: '剩余300字', trigger: 'change'}
                    ]
                }
            }
        },
        beforeRouteEnter(to, from, next) {
            next(vm => {
                vm.getProductType() //获取产品分类
                vm.getWarehouse()　　//获取仓库下拉
            });
        },
        mounted(){
        },
        methods: {
            alert(){
                console.log(this.form.reason)
            },
            //获取产品分类
            getProductType () {
                let _this = this
                _this.$indexServer.getCateTree().then(res => {
                    if (res.data.code == 200) {
                        _this.cateTree = res.data.data
                    }
                });
            },
            //获取产品规格
            getProductColor (id) {
                let _this = this
                _this.normAttrId = ''
                _this.$indexServer.getAttrsByProductId(id).then(res => {
                    if (res.data.code == 200) {
                        _this.productColor = res.data.data
                    }
                });
            },
            //根据商品id和属性id获取sku
            getProductSku () {
                let _this = this
                _this.$indexServer.getProductGood (_this.productId, _this.normAttrId).then(res => {
                    if (res.data.code == 200) {
                        _this.productSku = res.data.data.sku
                        _this.form.productGoodId = res.data.data.productGoodId
                    }
                });
            },
            // 清除二级和规格下拉的默认选项
            clearId(){
                this.cateTreeId = ''
                this.normAttrId = ''
            },
            //获取仓库下拉
            getWarehouse() {
                this.$indexServer.getWarehouse().then(res => {
                    if (res.data.code == 200) {
                        this.warehouse = res.data.data.warehouse;
                    }
                });
            },
            //完成出库，提交
            onSubmit() {
                let _this = this
                console.log(_this.form)
            },
            handleChange() {
                console.log(this.selectedOptions);
            },
            wordsNum () {
                console.log(this.form.remark.length)
            }
        }
    }
</script>
<style scoped>
    .box-top{
        font-size: 14px;
        color: #48576a;
        margin-bottom: 22px;
    }
    .box-top label{
        display: inline-block;
        width: 88px;
        padding: 11px 12px 11px 0;
        text-align: right;
    }
    .box-top input{
        padding: 3px 10px;
        height: 30px;
        line-height: 30px;
        min-width: 300px;
        margin-right: 20px;
        border: none;
        color: #1f2d3d;
    }
    .form-box {
        width: 850px;
    }
    .box_scan{
        font-size: 14px;
        width: 100%;
        overflow: hidden;
        margin-bottom: 22px;
    }
    .box_scan label{
        display: inline-block;
        margin-right: 12px;
    }
    .box_scan input{
        display: inline-block;
        border: none;
        width: 300px;
        color: #FF0000;
    }
</style>
