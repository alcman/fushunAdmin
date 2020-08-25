<template>
  <div class="container">
    <div class="title-virus">
      <span>疫情分析</span>
    </div>
    <div class="dateSelect">
      <el-date-picker
        v-model="value1"
        type="date"
        placeholder="选择日期"
        value-format="yyyyMMdd"
        class="datePick"
        @change="getRegionCount"
        :picker-options="pickerOptions"
      ></el-date-picker>
      <span>疫情统计</span>
    </div>
    <div class="table-count">
      <el-table
        :data="list"
        style="width: 100%"
        class="datatable"
        :row-style="{height:30+'px','text-align':'center'}"
        :cell-style="{padding:0+'px','text-align':'center',color:'white'}"
        :header-cell-style="{background:'#073c5f',color:'white'}"
      >
        <el-table-column
          label="地区"
          prop="region"
          align="center"
          width="90"
          :formatter="formatRegion"
        ></el-table-column>
        <el-table-column label="确诊" prop="diagnosed" align="center" width="90"></el-table-column>
        <el-table-column label="疑似" prop="suspected" align="center" width="90"></el-table-column>
        <el-table-column label="治愈" prop="cured" align="center" width="90"></el-table-column>
        <el-table-column label="死亡" prop="dead" align="center" width="90"></el-table-column>
        <el-table-column label="密切观察者" prop="contacts" align="center" width="90"></el-table-column>
      </el-table>
    </div>
    <div id="table-age"></div>
    <div class="table3">
      <div id="left"></div>
      <div id="right"></div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      value1: "",
      left: "",
      right: "",
      tableAge: "",
      AgeData: "",
      sourceData: [],
      AgeCount: [],
      observationData: [],
      list: [],
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now() - 8.64e6;
        },
      },
    };
  },
  mounted() {
    this.getObservation();
    this.getAgeCount();
    this.getSourceCount();
    this.getRegionCount2();
  },
  methods: {
    async getRegionCount2() {
      let res = await this.$axios({
        method: "get",
        url: `/api/analyse/getRegionCount`,
      });
      if (res.data.code === 200) {
        this.list = res.data.data;
      }
    },
    async getRegionCount(val) {
      let res = await this.$axios({
        method: "get",
        url: `/api/analyse/getRegionCount?date=${val}`,
        params: val,
      });
      console.log(val);
      if (res.data.code === 200) {
        this.list = res.data.data;
      }
    },
    async getAgeCount() {
      let res = await this.$axios({
        method: "get",
        url: "/api/analyse/getAgeCount",
      });
      if (res.data.code === 200) {
        // console.log(res.data.data);
        this.AgeData = res.data.data;
        let arr = Object.values(this.AgeData[0]);
        // console.log(arr.pop());
        this.AgeCount = arr.slice(0, arr.length - 1);
      }
      this.tableAge = this.$echarts.init(document.getElementById("table-age"));
      var option3 = {
        title: {
          text: "全市病例趋势统计",
          x: "center",
          y: 30,
          textStyle: {
            fontSize: 15,
            fontWeight: "bolder",
            color: "white", // 主标题文字颜色
          },
        },
        color: ["#43a0ef"],
        tooltip: {
          trigger: "axis",
          axisPointer: {
            // 坐标轴指示器，坐标轴触发有效
            type: "shadow", // 默认为直线，可选为：'line' | 'shadow'
          },
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true,
        },
        xAxis: [
          {
            type: "category",
            data: [
              "9岁以下",
              "10-19岁",
              "20-29岁",
              "30-39岁",
              "40-49岁",
              "50-59岁",
              "60-69岁",
              "70岁以上",
            ],
            axisLabel: {
              show: true,
              interval: 0, //控制是否全部显示
              textStyle: {
                color: "white", //更改坐标轴文字颜色
              },
            },
            axisLine: {
              lineStyle: {
                color: "white", //更改坐标轴颜色
              },
            },
            axisTick: {
              alignWithLabel: true,
            },
          },
        ],
        yAxis: [
          {
            type: "value",
            axisLabel: {
              show: true,
              textStyle: {
                color: "white", //更改坐标轴文字颜色
              },
            },
            axisLine: {
              lineStyle: {
                color: "white", //更改坐标轴颜色
              },
            },
          },
        ],

        series: [
          {
            type: "bar",
            barWidth: "60%",
            data: this.AgeCount,
          },
        ],
      };
      this.tableAge.setOption(option3);
    },
    async getSourceCount() {
      let res = await this.$axios({
        method: "get",
        url: "/api/analyse/getSourceCount",
      });
      if (res.data.code === 200) {
        this.sourceData = res.data.data;
        var dataName = [];
        // 定义一个数组，对话放到series里data里
        var dataPie = [];
        // 遍历对象，外层遍历标题
        for (let key in this.sourceData) {
          // 内层遍历具体的值
          for (let key2 in this.sourceData[key]) {
            // 定义一个对象(循环遍历)，最后放到数组里
            var obj = {};
            // 打印出具体name值
            // console.log(key2);
            // name值传到数组里
            obj.value = this.sourceData[key][key2];
            if (key2 == "local") {
              key2 = "本地";
            } else if (key2 == "input") {
              key2 = "输入";
            }
            dataName.push(key2);
            dataName.splice(2, 1);
            // 给对象的value，添加一个数据：'1','2'...
            // obj.value = this.sourceData[key][key2];
            // 给对象添加一个name
            obj.name = key2;
            // 最后这个对象添加到数据里
            dataPie.push(obj);
            dataPie.splice(2, 1);
          }
        }
        this.left = this.$echarts.init(document.getElementById("left"));
        var option = {
          title: {
            text: "确诊病例来源分析",
            left: "center",
            textStyle: {
              fontSize: 15,
              fontWeight: "bolder",
              color: "white", // 主标题文字颜色
            },
          },
          tooltip: {
            trigger: "item",
            formatter: "{a} <br/>{b} : {c} ({d}%)",
          },
          legend: {
            orient: "vertical",
            left: "left",
            y: "center",
            data: dataName,
            textStyle: {
              fontWeight: "bolder",
              color: "white", // 主标题文字颜色
            },
          },
          color: ["#12e2d1", "#48abff"],
          series: [
            {
              name: "访问来源",
              type: "pie",
              radius: "70%",
              center: ["50%", "60%"],
              label: {
                normal: {
                  position: "inner",
                  show: false,
                },
              },
              data: dataPie,
              emphasis: {
                itemStyle: {
                  shadowBlur: 10,
                  shadowOffsetX: 0,
                  shadowColor: "rgba(0, 0, 0, 0.5)",
                },
              },
            },
          ],
        };
        this.left.setOption(option);
      }
    },

    async getObservation() {
      let res = await this.$axios({
        method: "get",
        url: "/api/analyse/getObservationCount",
      });
      if (res.data.code === 200) {
        this.observationData = res.data.data;
        var dataName = [];
        // 定义一个数组，对话放到series里data里
        var dataPie = [];
        // 遍历对象，外层遍历标题
        for (let key in this.observationData) {
          // 内层遍历具体的值
          for (let key2 in this.observationData[key]) {
            // 定义一个对象(循环遍历)，最后放到数组里
            var obj = {};
            // 打印出具体name值
            // console.log(key2);
            // name值传到数组里
            obj.value = this.observationData[key][key2];
            if (key2 == "home") {
              key2 = "居家";
            } else if (key2 == "grouped") {
              key2 = "集中";
            }
            dataName.push(key2);
            dataName.splice(2, 1);
            // 给对象的value，添加一个数据：'1','2'...
            // obj.value = this.sourceData[key][key2];
            // 给对象添加一个name
            obj.name = key2;
            // 最后这个对象添加到数据里
            dataPie.push(obj);
            dataPie.splice(2, 1);
          }
        }
        this.right = this.$echarts.init(document.getElementById("right"));
        var option = {
          title: {
            text: "医学观察所分析",
            left: "center",
            textStyle: {
              fontSize: 15,
              fontWeight: "bolder",
              color: "white", // 主标题文字颜色
            },
          },
          tooltip: {
            trigger: "item",
            formatter: "{a} <br/>{b} : {c} ({d}%)",
          },
          color: ["#12e2d1", "#48abff"],
          series: [
            {
              name: "医学观察所分析",
              type: "pie",
              radius: ["40%", "70%"],
              center: ["50%", "60%"],
              data: dataPie,
              emphasis: {
                itemStyle: {
                  shadowBlur: 10,
                  shadowOffsetX: 0,
                  shadowColor: "rgba(0, 0, 0, 0.5)",
                },
              },
            },
          ],
        };
        this.right.setOption(option);
      }
    },

    formatRegion(row, column) {
      let type = row.region;
      if (type === 1) {
        return "全国";
      } else if (type === 2) {
        return "辽宁省";
      } else if (type === 3) {
        return "抚顺市";
      }
    },

    changeCellStyle(row) {
      //列的label的名称
      if (row.label === "确诊") {
        // return "color:pink"; //修改的样式
        console.log("111");
      } else {
        return "";
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.title-virus {
  color: white;
  span{
    margin-left: 0px;
  }
}
.dateSelect {
  margin-top: 10px;


  span {
    color: white;
    text-align: center;
  }
}
.datePick {
  width: 150px;
  height: 15px;
  .el-input__inner {
    background-color: transparent;
    border: transparent;
  }
}
.container {
  background-image: url(../static/2-bg.png);
  background-repeat: no-repeat;
}
.table-count {
  height: 166px;
  width: 583px;
  margin-top: 20px;
  text-align: center;
}
#table-age {
  height: 206px;
}
.table3 {
  height: 206px;
}
#left {
  width: 250px;
  height: 180px;
  float: left;
}
#right {
  width: 250px;
  height: 180px;
  display: inline-block;
}
// .datatable{
//     height: 100%;
//     width: 100%;
// }
/* 疫情统计表设置 */
.table-count /deep/ .el-table,
.el-table__expanded-cell {
  background-color: transparent;
}
.table-count /deep/ .el-table__row > td {
  /* 去除表格线 */
  border: none;
}
.table-count /deep/ .el-table th.is-leaf {
  /* 去除上边框 */
  border: none;
}
.table-count /deep/ .el-table::before {
  //去掉最下面的那一条线
  height: 0px;
}
.table-count /deep/ .el-table tr {
  background-color: transparent !important;
}
.table-count /deep/ .el-table--enable-row-transition .el-table__body td,
.el-table .cell {
  background-color: transparent;
}
.dateSelect /deep/ .el-input__inner {
  background-color: transparent;
  // border: #12e2d1;
}
</style>
