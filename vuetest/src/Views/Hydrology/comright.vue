<template>
    <div id="rightContent" v-bind:style="{height:rightheight+'px'}">
        <div class="topdiv2">
            <el-row class="topleft">
                <el-col v-bind:span="24" class="col">
                    <span class="divcss swzcss">水文站总数(座)：</span>
                    <span class="text-gra swzcss">{{czzs}}</span>
                </el-col>
            </el-row>
            <el-row type="flex" class="topicon" justify="center" align="middle">
                <el-col v-bind:span="2"> </el-col>
                <el-col v-bind:span="4">
                    <div style="float:left;"><img src="/static/Images/swsy/雨量站.png"></div>
                </el-col>
                <el-col v-bind:span="7" class="rightopicon">
                    <div class="cztjname">雨量站</div>
                    <div class="col cztjnum" style="margin-top: 1px !important;">11</div>
                </el-col>
                <el-col v-bind:span="4">
                    <div style="float:left;"><img src="/static/Images/swsy/水位站.png"></div>
                </el-col>
                <el-col v-bind:span="7" class="rightopicon">
                    <div class="cztjname">水位站</div>
                    <div class="col cztjnum" style="margin-top: 1px !important;">22</div>
                </el-col>
            </el-row>
            <el-row type="flex" class="topicon" justify="center" align="middle">
                <el-col v-bind:span="2"> </el-col>
                <el-col v-bind:span="4">
                    <div style="float:left;"><img src="/static/Images/swsy/水文站.png"></div>
                </el-col>
                <el-col v-bind:span="7" class="rightopicon">
                    <div class="cztjname">水文站</div>
                    <div class="col cztjnum" style="margin-top: 1px !important;">33</div>
                </el-col>
                <el-col v-bind:span="4">
                    <div style="float:left;"><img src="/static/Images/swsy/水质站.png"></div>
                </el-col>
                <el-col v-bind:span="7" class="rightopicon">
                    <div class="cztjname">水质站</div>
                    <div class="col cztjnum" style="margin-top: 1px !important;">44</div>
                </el-col>
            </el-row>
        </div>
        <div class="bardiv2" id="interval2">
            <div class="titlediv" style="width: 70px !important;left: 22px !important;">雨量统计</div>
            <div id="barCharts" class="barcss"></div>
        </div>
        <div class="tablediv">
            <div class="titlediv" style="left: 25px !important;">实时报警信息</div>
            <el-row class="headercss titlecss">
                <el-col v-bind:span="3">序号</el-col>
                <el-col v-bind:span="8">站点名称</el-col>
                <el-col v-bind:span="7">报警类型</el-col>
                <el-col v-bind:span="6">报警信息</el-col>
            </el-row>
            <div class="tablecss">
                <el-row class="titlecss" v-for="(realTime, index) in realTimedatas" v-bind:key="realTime.name">
                    <el-col v-bind:span="3" style="text-align:center;">{{index+1}}</el-col>
                    <el-col v-bind:span="8">{{realTime.name}}</el-col>
                    <el-col v-bind:span="7">{{realTime.type}}</el-col>
                    <el-col v-bind:span="6">{{realTime.value}}</el-col>
                </el-row>
            </div>
        </div>

    </div>
</template>
<script>
import $ from "jquery";
var echarts = require("echarts");
export default {
  data() {
    return {
      rightheight: document.documentElement.clientHeight - 30,
      barOption: null,
      CZdatas: [],
      realTimedatas: [],
      YLnames: [],
      YLnums: [],
      num: 0,
      town: "",
      czzs: 0,
      czxxnum: []
    };
  },
  props: {
    lefttown: {
      type: String,
      default: ""
    }
  },
  watch: {
    rightheight: function(val) {
      this.rightheight = val;
    },
    lefttown: function(val, oldval) {
      this.town = val;
      this.CZdatas.s_type = "1";
      this.CZdatas.count = "22";
    }
  },
  mounted() {
    const that = this;
    $("#interval2").css(
      "margin-top",
      (document.documentElement.clientHeight * 0.05 - 6) / 2
    );
    $(window).resize(function() {
      that.rightheight = document.documentElement.clientHeight - 30;
      $("#interval2").css(
        "margin-top",
        (document.documentElement.clientHeight * 0.05 - 6) / 2
      );
    });
    this.setSize();
    this.resize();
    this.init();
  },
  methods: {
    init: function() {
      this.getCZnum();
      this.getYLnum();
      this.getTimeInfo();
    },
    getCZnum: function() {
      this.czzs = 46;
    },
    getYLnum() {
      this.setBarData();
    },
    getTimeInfo(){

    },
    setBarData() {
      // 基于准备好的dom，初始化echarts实例
      var barChart = echarts.init(document.getElementById("barCharts"));
      var dataShadow = [];
      var yMax = 500;
      for (var i = 0; i < this.YLnames.length; i++) {
        dataShadow.push(yMax);
      }

      var barOption = {
        color: "#4a77ff",
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "shadow"
          }
        },
        dataZoom: [
          {
            yAxisIndex: 0,
            show: false,
            type: "slider", // 这个 dataZoom 组件是 slider 型 dataZoom 组件
            startValue: 0, // 从头开始。
            endValue:
              this.num > this.YLnames.length
                ? this.YLnames.length - 1
                : this.num - 1 // 一次性展示(num+1)个。
          }
        ],
        grid: {
          top: "3%",
          left: "3%",
          bottom: "9%",
          containLabel: true
        },
        xAxis: {
          name: "mm",
          nameLocation: "end",
          nameGap: "1",
          type: "value",
          nameTextStyle: {
            color: "#fff"
          },
          splitLine: { show: false }, //去除网格线
          axisTick: { show: false },
          axisLabel: {
            color: "#fff"
          },
          axisLine: {
            lineStyle: {
              color: "#3d4997"
            }
          }
        },
        yAxis: {
          type: "category",
          data: this.YLnames,
          axisTick: { show: false },
          axisLabel: {
            color: "#fff"
          },
          axisLine: {
            lineStyle: {
              color: "#3d4997"
            }
          }
        },
        series: [
          {
            // For shadow
            type: "bar",
            itemStyle: {
              normal: { color: "rgba(26,94,153,0.5)" }
            },
            barGap: "-100%",
            barCategoryGap: "40%",
            data: dataShadow,
            animation: false
          },
          {
            name: "",
            type: "bar",
            data: this.YLnums,
            label: {
              normal: {
                show: true,
                position: ["45%", "40%"],
                color: "#fff",
                fontSize: 14
              }
            }
          }
        ]
      };
      barChart.setOption(barOption);
      $(window).resize(
        function() {
          setTimeout(
            function() {
              barChart.resize();
            }.bind(this),
            100
          );
        }.bind(this)
      );

      setInterval(
        function() {
          var len = this.YLnames.length;
          if (barOption.dataZoom[0].endValue == len - 1) {
            // 每次向后滚动一个，最后一个从头开始。
            barOption.dataZoom[0].endValue =
              this.num > len ? len - 1 : this.num - 1;
            barOption.dataZoom[0].startValue = 0;
          } else {
            barOption.dataZoom[0].endValue = barOption.dataZoom[0].endValue + 1;
            barOption.dataZoom[0].startValue =
              barOption.dataZoom[0].startValue + 1;
          }
          barChart.setOption(barOption);
        }.bind(this),
        1000
      );
    },
    resize: function() {
      $(window).resize(
        function() {
          this.setSize();
        }.bind(this)
      );
    },
    setSize: function() {
      var docH = document.documentElement.clientHeight;
      var docW = document.documentElement.clientWidth;
      var contentH = docH - 30;
      this.height = contentH / 3;

      //分辨率不同 显示条数变化
      if (docH <= 768) {
        this.num = 4;
      } else if (768 < docH <= 960) {
        this.num = 7;
      } else if (960 < docH <= 1024) {
        this.num = 8;
      } else {
        this.num = 10;
      }
    }
  }
};
</script>
<style scoped>
#rightContent {
  width: 100%;
  overflow: hidden;
  min-width: 280px;
  /*min-height: 610px;*/
  position: relative;
}

@media screen and (min-height: 768px) {
  .rightopicon {
    margin-left: -10px;
    margin-top: 5px;
  }
  .topicon {
    height: 55px !important;
  }
  .tablediv {
    height: 40% !important;
    padding-top: 12px;
  }
  .topdiv2 {
    height: 20% !important;
  }
}
/*@@media screen and (max-width: 1366px) {
        .tablediv {
            height: 35%;
    }
        .topdiv2 {
            height: 25%;
        }
    }*/
.topleft {
  height: 35px !important;
}

.cztj {
  left: 35px !important;
  width: 70px !important;
}

.topicon {
  height: 43px;
}

#rightContent .col {
  height: 30px;
  margin-left: 10px;
  margin-top: 0px !important;
  line-height: 30px;
  text-align: center;
  width: 95%;
}

.marcss {
  margin-left: 0 !important;
}

.swzcss {
  float: none !important;
  text-align: center !important;
}

.text-gra {
  line-height: 30px !important;
}

.divcss {
  float: left;
  color: #ffffff !important;
  font-size: 14px !important;
  font-family: "microsoft yahei" !important;
}

.cztjname {
  color: #ffffff !important;
  font-size: 14px !important;
  font-family: "microsoft yahei" !important;
}

.cztjnum {
  color: #ffffff !important;
  font-size: 14px !important;
  font-family: Arial !important;
  margin-top: -10px;
  margin-left: 0 !important;
  text-align: left !important;
}

.barcss {
  height: 95%;
  width: 99%;
}

.tablediv {
  height: 40%;
  width: 99%;
  bottom: 0;
  position: absolute;
  border: 1px solid #2979e5;
}
.tablecss::-webkit-scrollbar {
  display: none;
}

.tablecss {
  width: 100%;
  height: 80%;

  overflow: hidden;
}

.tablecss:hover {
  overflow: auto;
}

.headercss {
  background-color: #2d3a8d;
  margin-top: 14px;
  text-align: center !important;
}

.titlecss {
  text-align: left;
  color: #ffffff;
  float: left;
  font-size: 14px;
  font-family: "Microsoft YaHei";
  height: 30px;
  padding-top: 8px;
  width: 100%;
  line-height: 15px;
}

.tabcss {
  color: #ffffff;
  float: left;
  font-size: 12px;
  font-family: "Microsoft YaHei";
  text-align: left;
  height: 45px;
  padding-top: 8px;
}

.topdiv2 {
  height: 25%;
  position: relative;
  border: 1px solid #2979e5;
  width: 99%;
}

.bardiv2 {
  height: 35%;
  position: relative;
  border: 1px solid #2979e5;
  width: 99%;
}
</style>


