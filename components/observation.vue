<template>
  <div class="container">
    <!-- <div class="medical">
      <el-tree
        :data="data"
        ref="tree"
        :props="defaultProps"
        current-node-key="1"
        node-key="id"
        default-expand-all
      >
        <div class="custom-tree-node" slot-scope="{ node}">
          <div>
            <span>{{ node.label }}{{node.num}}</span>
          </div>
        </div>
      </el-tree>
    </div>-->
    <div class="medical" v-for="val in this.medicalData" :key="val.date">
      <div>
        <span>全市定点医疗机构{{val.total}}个</span>
      </div>
      <div>
        <span>可用床位{{val.enable}}人</span>
      </div>
      <div>
        <span>已用床位{{val.used}}人</span>
      </div>
      <div>
        <span>剩余床位{{val.remain}}人</span>
      </div>
    </div>
    <div class="observation" v-for="val in this.observationData" :key="val.date">
      <div>
        <span>全市集中隔离医学观察所{{val.total}}个</span>
      </div>
      <div>
        <span>可容纳人数{{val.enable}}人</span>
      </div>
      <div>
        <span>已经容纳人数{{val.used}}人</span>
      </div>
      <div>
        <span>剩余可容纳人数{{val.remain}}人</span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      observationData: [],
      medicalData: [],
      medicalList: [{ avaliable: 10, used: 10, remain: 10 }],
      a: 10,
      data: [
        {
          label: "全市定点医疗机构",
          open: true,
          children: [
            {
              label: "可用床位",
              num: 10,
            },
            {
              label: "已用床位",
              num: 20,
            },
            {
              label: "剩余床位",
              num: 30,
            },
          ],
        },
      ],
      defaultProps: {
        children: "children",
        label: "label",
        num: "num",
      },
    };
  },
  mounted() {
    this.getObservation();
    this.getHospital()
  },
  methods: {
    async getObservation() {
      let res = await this.$axios({
        method: "get",
        url: "/api/medical/getObservation",
      });
      if (res.data.code === 200) {
        this.observationData = res.data.data;
      }
    },
    async getHospital() {
      let res = await this.$axios({
        method: "get",
        url: "/api/medical/getHospitals",
      });
      if (res.data.code === 200) {
        this.medicalData = res.data.data;
      }
    },
  },
};
</script>
<style lang="scss" scoped>
.medical {
  span {
    color: white;
  }
  div {
    margin-top: 10px;
    background-color: #073c5f;
  }
  width: 250px;
  float: left;
}
.observation {
  span {
    color: white;
  }
  div {
    margin-top: 10px;
    background-color: #073c5f;
  }
  width: 250px;
  float: left;
  margin-left: 30px;
}

// deep穿透
/deep/ .el-tree {
  background-color: transparent;
  color: white;
  // 将每一行的设置为相对定位 方便后面before after 使用绝对定位来固定位置
  .el-tree-node {
    position: relative;
    padding-left: 10px;
  }
  // 子集像右偏移 给数线留出距离
  .el-tree-node__children {
    padding-left: 10px;
  }
  //这是竖线
  .el-tree-node :last-child:before {
    height: 40px;
  }
  .el-tree > .el-tree-node:before {
    border-left: none;
  }
  .el-tree > .el-tree-node:after {
    border-top: none;
  }
  //这自定义的线 的公共部分
  .el-tree-node:before,
  .el-tree-node:after {
    content: "";
    left: -4px;
    position: absolute;
    right: auto;
    border-width: 1px;
  }
  .tree :first-child .el-tree-node:before {
    border-left: none;
  }
  // 竖线
  .el-tree-node:before {
    border-left: 1px solid #073c5f;
    bottom: 0px;
    height: 100%;
    top: -25px;
    width: 1px;
  }
  //横线
  .el-tree-node:after {
    border-top: 1px solid #073c5f;
    height: 20px;
    top: 14px;
    width: 24px;
  }
  .el-tree-node__expand-icon.is-leaf {
    width: 8px;
  }
  //去掉elementui自带的展开按钮  一个向下的按钮,打开时向右
  .el-tree-node__content > .el-tree-node__expand-icon {
    display: none;
  }
  //每一行的高度
  .el-tree-node__content {
    line-height: 30px;
    height: 30px;
  }
}
//去掉最上级的before  after 即是去电最上层的连接线
/deep/ .el-tree > div {
  &::before {
    display: none;
  }
  &::after {
    display: none;
  }
}
</style>