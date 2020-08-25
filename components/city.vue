<template>
  <div class="container">
    <div class="title">全市疫情情况</div>
    <div class="middle" v-for="val in all" :key="val.id">
      <span>确诊：{{val.diagnosed}}例</span>
      <span>疑似：{{val.suspected}}例</span>
      <span>治愈：{{val.cured}}例</span>
      <span>死亡：{{val.dead}}例</span>
    </div>
    <div class="tables">
      <div id="tableA"></div>
      <div id="tableB"></div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Echarts",
  data() {
    return {
      table1: "",
      table2: "",
      all: [],
      fourDays: [],

      contacts: [],
      diagnosedData: [],
      suspectedData: [],
      curedData: [],
      deadData: [],
      dateData: [],
      contactsData: [],
    };
  },
  methods: {
    async getVirus() {
      let res = await this.$axios({
        method: "get",
        url: "/api/virus/getCityVirus",
      });
      if (res.data.code === 200) {
        // console.log(res.data.data);
        this.all = res.data.data;
        // this.diagnosedData = this.TicketParams();
        // console.log(this.diagnosedData);
      }
    },
    async getFourDays() {
      let res = await this.$axios({
        method: "get",
        url: "/api/virus/getFourDays",
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
          return obj.cured;
        });
        let arr4 = this.fourDays.map((obj) => {
          return obj.dead;
        });
        let arr5 = this.fourDays.map((obj) => {
          return obj.date;
        });
        let arr6 = this.fourDays.map((obj) => {
          return obj.contacts;
        });
        this.diagnosedData = arr.reverse();
        this.suspectedData = arr2.reverse();
        this.curedData = arr3.reverse();
        this.deadData = arr4.reverse();
        this.dateData = arr5.reverse();
        this.contactsData = arr6.reverse();

        // this.echartsInit();
        this.table1 = this.$echarts.init(document.getElementById("tableA"));
        this.table2 = this.$echarts.init(document.getElementById("tableB"));
        var option = {
          title: {
            text: "全市病例趋势统计",
            x: "center",
            y: "15px",
            textStyle: {
              fontSize: 15,
              fontWeight: "bolder",
              color: "white", // 主标题文字颜色
            },
          },
          tooltip: {
            trigger: "axis",
          },
          //图例
          legend: {
            padding: [35, 0, 0, 0],
            data: ["确诊", "疑似", "治愈", "死亡"],
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
            data: ["07/18", "07/19", "07/20", "07/21"],
            // data:this.dateData,
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
              // data: [12, 13, 10, 13],
              data: this.diagnosedData,
            },
            {
              name: "疑似",
              type: "line",

              data: this.suspectedData,
            },
            {
              name: "治愈",
              type: "line",
              data: this.curedData,
            },
            {
              name: "死亡",
              type: "line",
              data: this.deadData,
            },
          ],
        };
        var option2 = {
          grid: {
            left: "9%",
            right: "5%",
            bottom: "10%",
            //   borderWidth: 10,
          },
          tooltip: {
            trigger: "axis",
          },
          title: {
            text: "全市密切观察者人数",
            x: "center",
            y: "15px",
            textStyle: {
              fontSize: 15,
              fontWeight: "bolder",
              color: "white", // 主标题文字颜色
            },
          },
          xAxis: {
            type: "category",
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
              data: this.contactsData,
              type: "line",
            },
          ],
        };
        this.table1.setOption(option);
        this.table2.setOption(option2);
      }
    },
  },
  mounted() {
    this.getFourDays();
    // this.echartsInit();
    // this.echartsInit2();
    this.getVirus();
  },
};
</script>
<style lang="scss" scoped>
#tableA {
  width: 250px;
  height: 180px;
  float: left;
  margin-left: 0px;
}
#tableB {
  width: 250px;
  height: 180px;
  // float: left;
  display: inline-block;
}

.container {
  background-image: url(../static/1-bg.png);
  background-repeat: no-repeat;
  width: 583px;
  height: 230px;
}
.title {
  text-align: center;
  display: inline-block;
  color: white;
}
.middle {
  text-align: center;
  color: white;
  margin-top: 10px;
}
</style>