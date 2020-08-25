<template>
  <div class="container">
    <div class="titles">
      <span>抚顺市截止7月22日,报告新冠共计例，其中：</span>
    </div>
    <div class="first" v-for="val in all" :key="val.date">
      <span class="s1">
        确诊
        <span>{{val.diagnosed}}</span>
      </span>
      <span class="s2">
        疑似
        <span>{{val.suspected}}</span>
      </span>
      <span class="s3">
        治愈
        <span>{{val.cured}}</span>
      </span>
      <span class="s4">
        死亡
        <span>{{val.dead}}</span>
      </span>
    </div>
    <div class="second">
      <span class="s5" v-for="val in observationData" :key="val.date">
        定点医院
        <span>{{val.total}}</span>
      </span>
      <span class="s6" v-for="val in medicalData" :key="val.date">
        医学观察所
        <span>{{val.total}}</span>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return { all: [], observationData: [], medicalData: [] };
  },
  mounted() {
    this.getVirus();
    this.getObservation();
    this.getHospital();
  },
  methods: {
    //获取疫情情况
    async getVirus() {
      let res = await this.$axios({
        method: "get",
        url: "/api/virus/getCityVirus",
      });
      if (res.data.code === 200) {
        this.all = res.data.data;
      }
    },
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
.container {
  background-image: url(../static/6-bg.png);
  background-repeat: no-repeat;
}
.first {
  margin-left: 30px;
  color: white;
  margin-top: 20px;
  .s1 {
    background-image: url(../static/num-bg-1.png);
    background-repeat: no-repeat;
    font-size: 12px;
    span {
      font-size: 20px;
      margin-left: 5px;
    }
  }
  .s2 {
    background-image: url(../static/num-bg-1.png);
    background-repeat: no-repeat;
    font-size: 12px;
    span {
      font-size: 20px;
      margin-left: 5px;
    }
  }
  .s3 {
    background-image: url(../static/num-bg-1.png);
    background-repeat: no-repeat;
    font-size: 12px;
    span {
      font-size: 20px;
      margin-left: 5px;
    }
  }
  .s4 {
    background-image: url(../static/num-bg-1.png);
    background-repeat: no-repeat;
    font-size: 12px;
    span {
      font-size: 20px;
      margin-left: 5px;
    }
  }
  span:not(:first-child) {
    margin-left: 130px;
  }
}
.second {
  margin-left: 170px;
  margin-top: 30px;
  color: white;
  .s5 {
    background-image: url(../static/num-bg-2.png);
    background-repeat: no-repeat;
    font-size: 12px;
    span {
      font-size: 20px;
      margin-left: 5px;
    }
  }
  .s6 {
    background-image: url(../static/num-bg-2.png);
    background-repeat: no-repeat;
    font-size: 12px;
    margin-left: 130px;
    span {
      font-size: 20px;
      margin-left: 5px;
    }
  }
}
.titles {
  text-align: center;
  color: white;
  // margin-top: 10px;
}
</style>