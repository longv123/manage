<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-date"></i> 库存管理</el-breadcrumb-item>
                <el-breadcrumb-item>商品入库</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class='storage_step'>
            <div class='step_1'>第一步：基本信息</div>
            <div class='step_2'>第二步：商品录入</div>
        </div>
        <div class="form-box">
            <el-form ref="form" :model="form" label-width="100px" :inline='true' :rules="rules">
                <el-form-item label="产品：" style='display: block'>
                    <el-cascader :options="options" v-model="selectedOptions" @change="handleChange"></el-cascader>
                </el-form-item>
                <el-form-item label="规格：">
                    <el-radio-group v-model="form.resource">
                        <el-radio label="中国红"></el-radio>
                        <el-radio label="传奇黑"></el-radio>
                        <el-radio label="樱花粉"></el-radio>
                        <el-radio label="经典白"></el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="商品SKU：">
                    <el-input v-model="form.warehouseId" :disabled='true'></el-input>
                </el-form-item>
                <el-form-item label="出库仓库：">
                    <el-select v-model="form.region" placeholder="请选择分区">
                        <el-option key="z" label="正常货品分区" value="z"></el-option>
                        <el-option key="f" label="返修货品分区" value="f"></el-option>
                        <el-option key="t" label="退货商品区" value="t"></el-option>
                        <el-option key="d" label="待检测分区" value="d"></el-option>
                        <el-option key="q" label="其他分区" value="q"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="出库原因：">
                    <el-select v-model="form.reason" placeholder="请选择出库原因">
                        <el-option key="af" label="APP购买订单发货" value="af"></el-option>
                        <el-option key="ff" label="【逸豆带你看世界】服务号发货" value="ff"></el-option>
                        <el-option key="gf" label="逸途旅行easyto公众号发货" value="gf"></el-option>
                        <el-option key="jf" label="APP金卡兑换订单发货" value="jf"></el-option>
                        <el-option key="zs" label="赠送" value="zs"></el-option>
                        <el-option key="hh" label="换货" value="hh"></el-option>
                        <el-option key="fb" label="返厂保修" value="fb"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="关系人信息" class='related' style='display: block'></el-form-item>
                <el-form-item label="姓名：" prop="name">
                    <el-input v-model="form.name" style='width: 120px'></el-input>
                </el-form-item>
                <el-form-item label="电话：" prop="phone" label-width='60px'>
                    <el-input v-model="form.phone" style='width: 150px'></el-input>
                </el-form-item>
                <el-form-item label="关联单号：" prop="orderId" label-width='90px'>
                    <el-input v-model="form.orderId" style='width: 271px'></el-input>
                </el-form-item>
                <el-form-item label="地址：" prop="address">
                    <el-input v-model="form.address" style='width: 730px'></el-input>
                </el-form-item>
                <el-form-item label="备注：">
                    <el-input type="textarea" v-model="form.remark" style='width: 730px'></el-input>
                </el-form-item>
                <el-form-item style='margin-left: 283px'>
                    <el-button style='width: 120px'>取消</el-button>
                    <el-button type="primary" @click="onSubmit('form')" style='width: 120px'>完成，下一步</el-button>
                </el-form-item>
            </el-form>
        </div>

    </div>
</template>

<script>
export default {
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
            options: [{
                value: '硬件产品',
                label: '硬件产品',
                children: [{
                    value: '一代翻译机',
                    label: '一代翻译机',
                }, {
                    value: '二代翻译机',
                    label: '二代翻译机',
                }]
            }],
            selectedOptions: [],
            rules: {
                remark: [
                    {required: true, max: 2, message: '剩余300字', trigger: 'change'}
                ]
            }
        }
    },
    methods: {
        onSubmit(formName) {
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    alert('submit!');
                } else {
                    console.log('error submit!!');
                    return false;
                }
            });
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
    .storage_step{
        overflow: hidden;
        margin-bottom: 22px;
    }
    .storage_step > div{
        font-size: 14px;
        float: left;
        color: #48576a;
        border: 1px solid #D1DBE5;
        background: #EEF1F6;
        height: 40px;
        line-height: 40px;
        width: 200px;
        text-align: center;
    }
    .storage_step .step_1{
        background: #20A0FF;
        border-color: #20A0FF;
        color: #fff;
        border-top-left-radius: 4px;
        border-bottom-left-radius: 4px;
    }
    .storage_step .step_2{
        border-top-right-radius: 4px;
        border-bottom-right-radius: 4px;
    }
    .el-cascader, .el-select, .el-input {
        width: 305px;
    }
    .form-box {
        width: 850px;
    }
</style>
