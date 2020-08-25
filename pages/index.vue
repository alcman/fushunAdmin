<template>
  <div class="containers">
    <div class="head">抚顺市战役一张图</div>
    <div class="all-tables">
      <div class="left-tables">
        <div class="left-one">
          <City></City>
        </div>
        <div class="left-two">
          <Analyse> </Analyse>
        </div>
      </div>
      <div class="middle-tables">
        <div class="totalCount">
          <Total></Total>
        </div>
        <div class="map"></div>
        <div class="bottom">
          <Condition></Condition>
        </div>
      </div>
      <div class="right-tables">
        <div class="right-one">
          <Increased></Increased>
        </div>
        <div class="right-two">
          <Observation></Observation>
        </div>
        <div class="right-three">
          <Materials></Materials>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import City from "@/components/city.vue";
import Increased from "@/components/increased.vue";
import Analyse from "@/components/virus-analyse.vue";
import Condition from "@/components/condition.vue";
import Total from "@/components/total.vue";
import Materials from "@/components/materials.vue";        
import Observation from "@/components/observation.vue";        
export default {
  components: {
    City,
    Increased,
    Analyse,
    Condition,
    Total,
    Materials,
  },
  data() {
    return {};
  },
};
</script>
<style>
.all-tables {
  width: 1840px;
  height: 850px;
  margin-left: 25px;
  display: inline-block;
}
.left-tables {
  width: 583px;
  height: 850px;
  float: left;
}
.left-one {
  width: 583px;
  height: 230px;
}
.middle-tables {
  width: 674px;
  height: 850px;

  float: left;
}
.right-tables {
  width: 583px;
  height: 850px;
  float: right;
}

.containers {
  background-image: url(../static/bg.png);
  height: 965px;
}
.city-table {
  width: 600px;
  height: 242px;
  /* background-color: aqua; */
  float: left;
}
.head {
  text-align: center;
  background-image: url(../static/title-bg.png);
  background-repeat: no-repeat;
  background-position: center;
  font-family: "FZZYJW";
  font-style: italic;
  font-size: 40px;
  color: white;
}
/* .increased-table {
  width: 600px;
  height: 242px;
  background-color: cornsilk;
  float: right;
} */
.right-one {
  height: 230px;
  color: white;
}
.right-two {
  height: 210px;
}
.right-three {
  height: 420px;
}

.totalCount {
  height: 200px;
}
.map {
  background-image: url(../static/map.png);
  background-repeat: no-repeat;
  background-size: contain;
  height: 450px;
  width: 674px;
  
}
.bottom {
  height: 200px;
  background-image: url(../static/7-bg.png);
  background-repeat: no-repeat;
}
</style>