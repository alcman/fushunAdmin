<template>
  <div class="container">
    <span class="title">全市疫情防控物资情况</span>
    <div class="sbutitle">
      <span class="s1">防控物资需求</span>
    </div>
    <div class="materials-table">
      <el-table
        :data="tableData"
        style="width:100%"
        :row-style="{height:30+'px','text-align':'center'}"
        :cell-style="{padding:0+'px','text-align':'center',color:'white'}"
        :header-cell-style="{background:'#073c5f',color:'white'}"
      >
        <el-table-column prop="type" label="物资种类" width="140" align="center"></el-table-column>
        <el-table-column prop="count" label="库存数量" width="140" align="center"></el-table-column>
        <el-table-column prop="needs" label="急需市级调配数量" align="center"></el-table-column>
        <el-table-column prop="replenish" label="昨日补充数量" align="center"></el-table-column>
      </el-table>
    </div>
    <div class="materials-give">
      <div class="title2">
        <span>物资发放情况</span>
      </div>
      <div class="giveDatas">
        <ul>
          <li v-for="val in giveData" :key="val.id">累计分发{{val.type}}{{val.count}}{{val.unit}}</li>
        </ul>
      </div>
      <!-- <div style="color:white">
        累计分发口罩
        <span style="float:right">{{ maskgive}}万只</span>
      </div>
      <div style="color:white">
        累计分发防护服
        <span style="float:right">{{ maskgive}}件</span>
      </div>
      <div style="color:white">
        累计分发手持测温仪
        <span style="float:right">{{ maskgive}}支</span>
      </div>
      <div style="color:white">
        累计分发消毒液
        <span style="float:right">{{ maskgive}}公升</span>
      </div>-->
    </div>
    <div class="materials-buy">
      <div class="title3">
        <span>物资采购情况</span>
      </div>
      <div class="sbutitle2">
        <span>国内采购</span>
        <span>国外采购</span>
      </div>
      <div class="buy-table">
        <el-table
          :data="buyData"
          style="width:100%;font-size:12px"
          :row-style="{height:30+'px','text-align':'center'}"
          :cell-style="{padding:0+'px','text-align':'center',color:'white'}"
          :header-cell-style="{background:'transparent',color:'#3baece',height:'20px'}"
        >
          <el-table-column prop="type" width="80" align="center"></el-table-column>
          <el-table-column prop="internalBuyCount" label="总采购量" width="80" align="center"></el-table-column>
          <el-table-column prop="internalToCityCount" label="为全市采购" align="center"></el-table-column>
          <el-table-column prop="foreignBuyCount" label="总采购量" align="center"></el-table-column>
          <el-table-column prop="foreignToCityCount" label="为全市采购" align="center"></el-table-column>
        </el-table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      //分发口罩数量
      maskgive: 10,
      buyData: [],
      tableData: [],
      giveData: [],
    };
  },
  mounted() {
    this.getMaterials();
    this.getMaterialsBuy();
    this.getMaterialsGive();
  },
  methods: {
    async getMaterials() {
      let res = await this.$axios({
        method: "get",
        url: "/api/material/getAllMaterials",
      });
      if (res.data.code === 200) {
        // console.log(res.data.data);
        this.tableData = res.data.data;
      }
    },
    async getMaterialsBuy() {
      let res = await this.$axios({
        method: "get",
        url: "/api/material/getMaterialBuy",
      });
      if (res.data.code === 200) {
        // console.log(res.data.data);
        this.buyData = res.data.data;
      }
    },
    async getMaterialsGive() {
      let res = await this.$axios({
        method: "get",
        url: "/api/material/getMaterialGive",
      });
      if (res.data.code === 200) {
        // console.log(res.data.data);
        this.giveData = res.data.data;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  background-image: url(../static/5-bg.png);
  background-repeat: no-repeat;
}
.giveDatas {
  color: white;
}
ul {
  text-align: left;
  margin: 0;
  padding: 0;
}
li {
  list-style-type: none;
  line-height: 35px;
}
.title {
  color: white;
}
.title2 {
  text-align: center;
  color: white;
}
.sbutitle {
  text-align: center;
  .s1 {
    color: white;
  }
}
.sbutitle2 {
  font-size: 12px;
  margin-left: 130px;
  span {
    float: left;
    display: block;
    text-align: center;
    width: 100px;
    background-color: #073c5f;
  }
}
.materials-give {
  float: left;
  font-size: 12px;
  width: 160px;
  div {
    margin-top: 25px;
  }
}
.materials-buy {
  width: 420px;
  color: white;
  float: left;
}
.title3 {
  text-align: center;
  color: white;
}
/* 防控物资需求表设置 */
.materials-table /deep/ .el-table,
.el-table__expanded-cell {
  background-color: transparent;
}
.materials-table /deep/ .el-table__row > td {
  /* 去除表格线 */
  border: none;
}
.materials-table /deep/ .el-table th.is-leaf {
  /* 去除上边框 */
  border: none;
}
.materials-table /deep/ .el-table::before {
  //去掉最下面的那一条线
  height: 0px;
}
.materials-table /deep/ .el-table tr {
  background-color: transparent !important;
}
.materials-table /deep/ .el-table--enable-row-transition .el-table__body td,
.el-table .cell {
  background-color: transparent;
}
/* 物资采购情况表设置 */
.materials-buy /deep/ .el-table,
.el-table__expanded-cell {
  background-color: transparent;
}
.materials-buy /deep/ .el-table__row > td {
  /* 去除表格线 */
  border: none;
}
.materials-buy /deep/ .el-table th.is-leaf {
  /* 去除上边框 */
  border: none;
}
.materials-buy /deep/ .el-table::before {
  //去掉最下面的那一条线
  height: 0px;
}
.materials-buy /deep/ .el-table tr {
  background-color: transparent !important;
}
.materials-buy /deep/ .el-table--enable-row-transition .el-table__body td,
.el-table .cell {
  background-color: transparent;
}
.materials-buy /deep/ .el-table__header tr,
.el-table__header th {
  padding: 0;
  height: 20px;
}
</style>