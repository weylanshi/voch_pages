<template>
    <div class="table">
        <div class="container">
            <div class="handle-box">
                <el-input v-model="reqData.cBalanceID" placeholder="合同结算单编码" class="handle-input mr10"></el-input>
                <!-- <el-input v-model="reqData.date1" placeholder="执行单日期1" class="handle-input mr10"></el-input> -->
                <!-- <el-input v-model="reqData.date2" placeholder="执行单日期2" class="handle-input mr10"></el-input> -->
                 <el-date-picker
                    v-model="reqData.date1"
                    align="right"
                    type="date"
                    value-format='yyyy-MM-dd'
                    placeholder="执行单日期1"
                  >
                   </el-date-picker>
                  <el-date-picker
                    v-model="reqData.date2"
                    align="right"
                    type="date"
                    value-format='yyyy-MM-dd'
                    placeholder="执行单日期2"
                  >
                  </el-date-picker>
                <el-button type="primary" size="mini" icon="search" @click="search">搜索</el-button>
                <el-button type="primary" size="mini" icon="search" @click="search">生成凭证</el-button>
            </div>
            <el-table :data="content" border  ref="multipleTable" >
                <!-- <el-table-column type="selection" width="55"></el-table-column> -->
                <!-- <el-table-column prop="guid" label="guid"  width="150">
                </el-table-column> -->
                <el-table-column prop="cBalanceID" label="结算单编码">
                </el-table-column>
                <el-table-column prop="strContractId" label="合同编码">
                </el-table-column>
                <el-table-column prop="strContractKind" label="合同类型分类">
                </el-table-column>
                <el-table-column prop="strContractName" label="合同名称" width="220">
                </el-table-column>
                <el-table-column prop="cTypeName" label="合同类型">
                </el-table-column>
                <el-table-column prop="strWay" label="收支方向">
                </el-table-column>
                <el-table-column prop="strBisectionUnit" label="对方单位编码">
                </el-table-column>
                <el-table-column prop="cCusName" label="对方单位名称" >
                </el-table-column>
                <el-table-column prop="strCurrency" label="币种">
                </el-table-column>
                <el-table-column prop="dblExchange" label="汇率" >
                </el-table-column>
                <el-table-column prop="dblTotalCurrency" label="结算单总金额" >
                </el-table-column>
                <el-table-column prop="dtCreateTime" label="制单日期" >
                </el-table-column>
                <!-- <el-table-column prop="address" label="地址" :formatter="formatter">
                </el-table-column>
                <el-table-column label="操作" width="180">
                    <template slot-scope="scope">
                        <el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                        <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                    </template>
                </el-table-column> -->
            </el-table>
            <div class="pagination">
                <el-pagination @current-change="handleCurrentChange" 
                    @size-change="handleSizeChange"
                     layout="total, sizes, prev, pager, next, jumper" 
                    :page-count="totalPages" :page-size="reqData.pageSize"  >
                </el-pagination>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: "basetable",
  data() {
    return {
      reqData: {
        pageNum: 1,
        pageSize: 10,
        cBalanceID: "",
        date1: "",
        date2: ""
      },
      content: [
        {
          cBalanceID: '',
          strContractId: '',
          strContractKind: "",
          strContractName: "",
          cTypeName: "",
          strWay: "",
          cCusName: "",
          strCurrency: "",
          dblExchange: "",
          dblTotalCurrency: "",
          dtCreateTime: ""
        }
      ],
      totalPages:0
    };
  },
  created() {
    this.getData();
  },
  computed: {
  },
  methods: {
    // 分页导航
    handleCurrentChange(val) {
      this.reqData.pageNum = val;
      this.getData();
    },
    handleSizeChange(val){
      this.reqData.pageSize = val;
      this.getData();
    },
    getData() {
      this.$axios
        .post("/api/JSD/outList", this.$qs.stringify(this.reqData))
        .then(res => {
          this.content = res.data.content;
          this.totalPages = res.data.totalPages;
        });
    },
    search() {
      this.reqData.pageNum=1;
      this.getData()
    }
   
  }
};
</script>

<style scoped>
.handle-box {
  margin-bottom: 20px;
}

.handle-select {
  width: 120px;
}

.handle-input {
  width: 300px;
  display: inline-block;
}
.del-dialog-cnt {
  font-size: 16px;
  text-align: center;
}
.pagination{
   text-align: center;
}
</style>
