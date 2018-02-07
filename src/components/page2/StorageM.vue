<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i> 库存管理</el-breadcrumb-item>
                <el-breadcrumb-item>出库明细</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="table_title">

        </div>
        <div class="table_nav">
            <h2>商品SN录入<span>*</span>:</h2>
            <p>默认扫码录入,请连接扫码枪，进行扫码录入。</p>
        </div>
        <div class="table_container">
           <v-table
            is-horizontal-resize
            style="width:23%"
            :columns="columns"
            :table-data="tableData"
            row-hover-color="skyblue"
             :cell-edit-done="cellEditDone"
            title-bg-color='#f4f4f4'
            >
            </v-table>
           <v-table
            is-horizontal-resize
            style="width:23%"
            :columns="columns"
            :table-data="tableData2"
            row-hover-color="skyblue"
             :cell-edit-done="cellEditDone"
            title-bg-color='#f4f4f4'
           ></v-table>
                <v-table
            is-horizontal-resize
            style="width:23%"
            :columns="columns"
            :table-data="tableData3"
            row-hover-color="skyblue"
             :cell-edit-done="cellEditDone"
            title-bg-color='#f4f4f4'
           ></v-table>
                <v-table
            is-horizontal-resize
            style="width:23%"
            :columns="columns"
            :table-data="tableData4"
            row-hover-color="skyblue"
             :cell-edit-done="cellEditDone"
            title-bg-color='#f4f4f4'
           ></v-table>
    </div>
    <div class="table_footer">
        <el-button type="info" plain>上一步</el-button>
       <el-button type="primary" plain>完成入库</el-button>

    </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            tableData3: [{ "id": "21", "sn": "156*****1987" },
            { "id": "22", "sn": "182*****1538" },
            { "id": "23", "sn": "161*****0097" },
            { "id": "24", "sn": "197*****1123" },
            { "id": "25", "sn": "183*****6678" },
            { "id": "26", "sn": "183*****6678" },
            { "id": "27", "sn": "183*****6678" },
            { "id": "28", "sn": "183*****6678" },
            { "id": "29", "sn": "183*****6678" },
            { "id": "30", "sn": "183*****6678" },],
            tableData4: [{ "id": "31", "sn": "156*****1987" },
            { "id": "32", "sn": "182*****1538" },
            { "id": "33", "sn": "161*****0097" },
            { "id": "34", "sn": "197*****1123" },
            { "id": "35", "sn": "183*****6678" },
            { "id": "36", "sn": "183*****6678" },
            { "id": "37", "sn": "183*****6678" },
            { "id": "38", "sn": "183*****6678" },
            { "id": "39", "sn": "183*****6678" },
            { "id": "40", "sn": "183*****6678" },],
            tableData2: [{ "id": "11", "sn": "156*****1987" },
            { "id": "12", "sn": "182*****1538" },
            { "id": "13", "sn": "161*****0097" },
            { "id": "14", "sn": "197*****1123" },
            { "id": "15", "sn": "183*****6678" },
            { "id": "16", "sn": "183*****6678" },
            { "id": "17", "sn": "183*****6678" },
            { "id": "18", "sn": "183*****6678" },
            { "id": "19", "sn": "183*****6678" },
            { "id": "20", "sn": "183*****6678" },],
            tableData: [
                { "id": "1", "sn": "156*****1987" },
                { "id": "2", "sn": "182*****1538" },
                { "id": "3", "sn": "161*****0097" },
                { "id": "4", "sn": "197*****1123" },
                { "id": "5", "sn": "183*****6678" },
                { "id": "6", "sn": "183*****6678" },
                { "id": "7", "sn": "183*****6678" },
                { "id": "8", "sn": "183*****6678" },
                { "id": "9", "sn": "183*****6678" },
                { "id": "10", "sn": "183*****6678" },
            ],
            columns: [
                {                    field: 'id', title: '序号', width: 80, titleAlign: 'center', columnAlign: 'center', isEdit: true,
                    formatter: function (rowData, rowIndex, pagingIndex, field) {
                        this.rowIndex = rowIndex
                        return `<span class='cell-edit-color'>${rowData[field]}</span>`;
                    }, isResize: true                },
                { field: 'sn', title: '商品SN码', width: 150, titleAlign: 'center', columnAlign: 'center', isEdit: true, isResize: true }
            ]
        }
    },
    created() {
        // this.getData();
    },
    computed: {
        // data() {
        //     const self = this;
        //     return self.tableData.filter(function (d) {
        //         let is_del = false;
        //         for (let i = 0; i < self.del_list.length; i++) {
        //             if (d.name === self.del_list[i].name) {
        //                 is_del = true;
        //                 break;
        //             }
        //         }
        //         if (!is_del) {
        //             if (d.address.indexOf(self.select_cate) > -1 &&
        //                 (d.name.indexOf(self.select_word) > -1 ||
        //                     d.address.indexOf(self.select_word) > -1)
        //             ) {
        //                 return d;
        //             }
        //         }
        //     })
        // },

    },
    methods: {

        // 单元格编辑回调
        cellEditDone(newValue, oldValue, rowIndex, rowData, field) {
            console.log(newValue, oldValue, rowData, rowIndex, field)
            this.tableData[rowIndex][field] = newValue;

            // 接下来处理你的业务逻辑，数据持久化等...
        },
        getData() {
            let self = this;
            // if (process.env.NODE_ENV === 'development') {
            //     self.url = '/ms/table/list';
            // };
        }
    }
}
</script>

<style scoped>
.table_title {
  height: 100px;
  background-color: #f4f4f4;
  margin-bottom: 40px;
}
.table_container {
  display: flex;
  justify-content: space-around;
}
.table_nav {
  display: flex;
  text-align: center;
  align-items: center;
  margin-bottom: 40px;
}
.table_nav span {
  color: red;
  margin-right: 10px;
}
.table_nav p {
  color: red;
  margin-left: 30px;
}
.table_footer {
  margin-top: 40px;
  margin-left: 20px;
}
</style>
