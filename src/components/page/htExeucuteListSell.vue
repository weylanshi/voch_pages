<template>
    <div class="table">
        <div class="container">
            <div class="handle-box">
                <el-input v-model="reqData.cExecID" placeholder="合同执行单编码" class="handle-input mr10" style="width:260px"></el-input>
                <el-input v-model="reqData.cContractID" placeholder="合同编码" class="handle-input mr10"></el-input>
                <el-input v-model="reqData.strContractName" placeholder="合同名称" class="handle-input mr10"></el-input>
                <el-date-picker
                    v-model="reqData.date1"
                    align="right"
                    type="date"
                    value-format='yyyy-MM-dd'
                    placeholder="制单日期最小日期"
                  >
                  </el-date-picker>
                  <el-date-picker
                    v-model="reqData.date2"
                    align="right"
                    type="date"
                    value-format='yyyy-MM-dd'
                    placeholder="制单日期最大日期"
                  >
                  </el-date-picker>
                     <br>
                <!-- <el-input v-model="reqData.date1" placeholder="制单日期最小日期" class="handle-input mr10"></el-input>
                <el-input v-model="reqData.date2" placeholder="制单日期最大日期" class="handle-input mr10"></el-input> -->
                <div class="btnGroup">
                  <el-button type="primary" size="mini" icon="search" @click="search">搜索</el-button>
                  <el-button type="primary" size="mini" icon="search" @click="search">生成凭证</el-button>
                </div>
            </div>
            <el-table :data="content" border  ref="multipleTable" empty-text="暂无数据">
                <el-table-column prop="cExecID" label="合同执行单编码">
                </el-table-column>
                <el-table-column prop="cContractID" label="合同编码">
                </el-table-column>
                <el-table-column prop="dtProduceDate" label="制单日期">
                </el-table-column>
                <el-table-column prop="strContractName" label="合同名称">
                </el-table-column>
                <el-table-column prop="strContractKind" label="合同类型" >
                </el-table-column>
                <el-table-column prop="decRateMoney" label="执行单总金额">
                </el-table-column>
          
                <!-- <el-table-column prop="address" label="地址" :formatter="formatter">
                </el-table-column>
                <el-table-column label="操作" width="180">
                    <template slot-scope="scope">
                        <el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                        <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                    </template>
                </el-table-column> -->
                 <el-table-column label="操作" width="100">
                    <template slot-scope="scope">
                        <el-button size="small" type="primary" :disabled="checkDefine(content[scope.$index].cDefine10)" @click="pzadd(content[scope.$index].guid)">生成凭证</el-button>
                    </template>
                </el-table-column> 
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
        cExecID: "",
        cContractID: "",
        date1: "",
        date2: "",
        strContractName: ""
      },
      content: [
        {
          dblExchange: 0,
          dblTotalCurrency: 0,
          guid: "",
          cExecID:"",
          cContractID:"",
	        dtProduceDate:"",
	        strContractName	:"",
	        strContractKind:"",
	        decRateMoney:""
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
        .post("/api/ZXD/inList", this.$qs.stringify(this.reqData))
        .then(res => {
          this.content = res.data.content;
          this.totalPages = res.data.totalPages;
        });
    },
    search() {
      this.reqData.pageNum=1;
      this.getData()
    },
    checkDefine(val){
        if(val){
            return true
        }
        return false
    },
    pzadd(guid) {
      let reqData = {
        guid: guid,
        type: 1
      };
      this.$axios
        .post("/api/PZ/pzAdd", this.$qs.stringify(reqData))
        .then(res => {
          res.pzId;
          if (!res.pzId) {
            this.$message.error("生成凭证失败");
          } else {
            this.$message.success("生成凭证成功");
            this.getData()
          }
        });
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
.pagination { 
  text-align: center;
}
.btnGroup{
  float: right;
  margin: 20px 154px;
  clear: both;
}
</style>
