<template>
  <div class="container">
    <div class="in">
      <div>
        <span style="font-size:17px">入户走访情况</span>
      </div>
      <div v-for="val in interviewCount" :key="val.id">
        <span>1月23日后湖北籍来本市人员{{val.fromHubei}}人</span>
        <span>1月23日后湖北返乡{{val.fromHome}}人</span>
      </div>
    </div>
    <div class="train" v-for="val in trainViewCount" :key="val.date">
      <div>
        <span style="font-size:17px">火车站出入境情况</span>
      </div>
      <div>
        <span>下车人数{{val.total}}人</span>
        <span>外省到抚顺市{{val.toFushun}}人</span>
        <span>湖北到抚顺市{{val.hubeiToFushun}}人</span>
        <span>武汉到抚顺市{{val.wuhanToFushun}}人</span>
      </div>
      <div>
        <span>体温异常{{val.temUnusual}}人</span>
        <span>留治观察{{val.remained}}人</span>
      </div>
    </div>
    <div class="road" v-for="val in roadViewCount" :key="val.id">
      <div>
        <span style="font-size:17px">入境公路防疫情况</span>
      </div>
      <div>
        <span>入境公路检查车辆{{val.carCount}}辆</span>
        <span>入境湖北籍车辆{{val.carHubeiCount}}辆</span>
        <span>入境公路检查人数{{val.personCount}}人</span>
        <span>入境湖北籍人数{{val.personHubeiCount}}人</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      interviewCount: [],
      trainViewCount: [],
      roadViewCount:[],
    };
  },
  mounted() {
    this.getInterviewCount();
    this.getTrainViewCount();
    this.getRoadViewCount()
  },
  methods: {
    async getInterviewCount() {
      let res = await this.$axios({
        method: "get",
        url: "/api/Station/getInterviewCounts",
      });
      if (res.data.code === 200) {
        this.interviewCount = res.data.data;
      }
    },
    async getTrainViewCount() {
      let res = await this.$axios({
        method: "get",
        url: "/api/Station/getTrainInterview",
      });
      if (res.data.code === 200) {
        this.trainViewCount = res.data.data;
      }
    },
    async getRoadViewCount() {
      let res = await this.$axios({
        method: "get",
        url: "/api/Station/getRoadInterview",
      });
      if (res.data.code === 200) {
        this.roadViewCount = res.data.data;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  color: white;
}
// .in{
//   margin-top: 10px;
// }
span {
  font-size: 12px;
  margin: 25px;
  line-height: 25px;
}
.train {
  margin-top: 10px;
}

.road {
  margin-top: 10px;
}
</style>