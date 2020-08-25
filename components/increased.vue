<template>
  <div class="container">
    <!-- <div class="title">新增疫情及分析</div>
    <div class="data">确诊：例，疑似：例，治愈：例，死亡：例，密切接触者：例</div>-->
    <div class="tables">
      <div class="title">新增疫情及分析</div>
      <div
        class="data"
        v-for="val in this.increasedData"
        :key="val.id"
      >确诊：{{val.diagnosed}}例，疑似：{{val.suspected}}例，治愈：{{val.cured}}例，死亡：{{val.dead}}例，密切接触者：{{val.contacts}}例</div>
      <div id="table1"></div>
      <div id="table2"></div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      table1: "",
      table2: "",
      increasedData: [],
      arr: [],
      fourDays: [],
      diagnosedData: [],
      suspectedData: [],
      totalData: [],
      dateData: [],
    };
  },
  mounted() {
    this.getIncreased();
    this.getFourDays();
    // this.echartsInit2();
  },
  methods: {
    async getIncreased() {
      let res = await this.$axios({
        method: "get",
        url: "/api/increased/getVirusIncreased",
      });
      if (res.data.code === 200) {
        this.increasedData = res.data.data;
        var dataName = [];
        var dataPie = [];
        for (let key in this.increasedData) {
          // console.log(this.increasedData[key]);
          // 内层遍历具体的值
          for (let key2 in this.increasedData[key]) {
            // 定义一个对象(循环遍历)，最后放到数组里
            var obj = {};
            obj.value = this.increasedData[key][key2];
            if (key2 == "suspectedToDiagnosed") {
              obj.value = this.increasedData[key][key2];
              key2 = "疑似转确诊";
              dataPie.push(obj);
              obj.name = key2;
              dataName.push(key2);
            } else if (key2 == "increasedNew") {
              obj.value = this.increasedData[key][key2];
              key2 = "新增确诊";
              dataName.push(key2);
              obj.name = key2;
              dataPie.push(obj);
            }
          }
        }
        this.table2 = this.$echarts.init(document.getElementById("table2"));
        var option = {
          title: {
            text: "病例新增来源分析",
            x: "center",
            textStyle: {
              fontSize: 15,
              fontWeight: "bolder",
              color: "white", // 主标题文字颜色
            },
          },
          tooltip: {
            trigger: "item",
            formatter: "{a} <br/>{b}: {c} ({d}%)",
          },
          color: ["#12e2d1", "#48abff"],
          series: [
            {
              name: "来源",
              type: "pie",
              radius: ["40%", "70%"],
              center: ["50%", "60%"],
              avoidLabelOverlap: false,
              label: {
                show: false,
                position: "center",
              },
              emphasis: {
                label: {
                  show: true,
                  fontSize: "10",
                  fontWeight: "bold",
                },
              },
              labelLine: {
                show: false,
              },
              data: dataPie,
            },
          ],
        };
        this.table2.setOption(option);
      }
    },
    async getFourDays() {
      let res = await this.$axios({
        method: "get",
        url: "/api/increased/getFourDays",
      });
      if (res.data.code === 200) {
        this.fourDays = res.data.data;
        let arr = this.fourDays.map((obj) => {
          return obj.diagnosed;
        });
        let arr2 = this.fourDays.map((obj) => {
          return obj.suspected;
        });
        let arr3 = this.fourDays.map((obj) => {
          return obj.total;
        });
        let arr4 = this.fourDays.map((obj) => {
          return obj.date;
        });
        this.diagnosedData = arr.reverse();
        this.suspectedData = arr2.reverse();
        this.totalData = arr3.reverse();
        this.dateData = arr4.reverse();
      }
      this.table1 = this.$echarts.init(document.getElementById("table1"));
      var option={
        title: {
          text: "全市病例趋势统计",
          x: "center",
          textStyle: {
            fontSize: 15,
            fontWeight: "bolder",
            color: "white", // 主标题文字颜色
          },
        },
        tooltip: {
          trigger: "axis",
        },
        legend: {
          padding: [35, 0, 0, 0],
          data: ["确诊", "疑似", "累计"],
          textStyle: {
            color: "white",
          },
        },
        grid: {
          left: "3%",
          right: "5%",
          bottom: "0.2%",
          containLabel: true,
        },
        xAxis: {
          type: "category",
          boundaryGap: false,
          data: this.dateData,
          axisLabel: {
            show: true,
            textStyle: {
              color: "#c3dbff", //更改坐标轴文字颜色
            },
          },
          axisLine: {
            lineStyle: {
              color: "white", //更改坐标轴颜色
            },
          },
        },
        yAxis: {
          type: "value",
          axisLabel: {
            show: true,
            textStyle: {
              color: "#c3dbff", //更改坐标轴文字颜色
            },
          },
          axisLine: {
            lineStyle: {
              color: "white", //更改坐标轴颜色
            },
          },
        },
        series: [
          {
            name: "确诊",
            type: "line",
            data: this.diagnosedData,
          },
          {
            name: "疑似",
            type: "line",

            data:this.suspectedData,
          },
          {
            name: "累计",
            type: "line",
            data: this.totalData,
          },
        ],
      }
      this.table1.setOption(option)
    },

  },
};
</script>

<style lang="scss" scoped>
#table1 {
  width: 259px;
  height: 180px;
  float: left;
  // display: inline-block;
}
#table2 {
  width: 250px;
  height: 180px;
  // float: left;
  display: inline-block;
}
.tables {
  background-image: url(../static/3-bg.png);
  background-repeat: no-repeat;
}
</style>