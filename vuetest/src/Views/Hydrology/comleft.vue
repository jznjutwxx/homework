<template>
    <div id="leftContent" v-bind:style="{height:leftheight+'px'}">
        <div class="topdiv">
            <el-row class="topleft">
                <el-col v-bind:xs="12" v-bind:sm="14" v-bind:md="17" v-bind:lg="18" v-bind:xl="19" class="col">
                    <div class="divcss">河道总数(条)：</div>
                    <div class="text-gra">4598</div>
                </el-col>
                <el-col v-bind:xs="12" v-bind:sm="10" v-bind:md="6" v-bind:lg="5" v-bind:xl="4" style="margin-top:6px;line-height:30px;">
                    <span class="xzqh">{{lefttown}}</span>
                </el-col>
                <el-col v-bind:span="24" class="col" style="margin-top: 2px !important;height:30px;">
                    <div class="divcss">重污染河道(条)：</div>
                    <div class="text-gra">115</div>
                </el-col>

                <el-col v-bind:span="8" style="margin-top: 2px !important;height:30px;">
                    <div class="colcss" style="margin-left:10px;">合格:</div>
                    <div class="colcss numcss"> 220条</div>
                </el-col>
                <el-col v-bind:span="8" style="margin-top: 2px !important;height:30px;">
                    <div class="colcss">不合格:</div>
                    <div class="colcss numcss"> 112条</div>
                </el-col>
                <el-col v-bind:span="8" style="margin-top: 2px !important;height:30px;">
                    <div class="colcss">合格率:</div>
                    <div class="colcss numcss"> 62.7%</div>
                </el-col>
            </el-row>
        </div>
        <div class="piediv" id="interval">
            <div class="titlediv szybdiv">水质月报(12月)</div>
            <compie v-bind:option="pieOption" class="piecss"></compie>
            <div class="centerdiv">
                <div>根据有机物污染综合评价</div>
                <div class="paddingcss">A值法(各断面均值)</div>
                <div class="paddingcss">同比：5.2%<span style="margin-left:5px;"><img src="/static/Images/szysy/ico_zengz.png"></span></div>
                <div class="paddingcss">环比：2.2%<span style="margin-left:5px;"><img src="/static/Images/szysy/ico_xiaj.png"></span></div>
            </div>
        </div>
        <div class="bardiv">
            <div class="titlediv szdiv" v-on:click="getFaterSzdbl">清洁水行动水质达标率(12月)</div>
            <combar v-bind:option="barOption" ref="bar" class="barcss"></combar>
        </div>
    </div>
</template>
<script>
import $ from "jquery";
import combar from "../../components/com_echarts/Bar";
import compie from "../../components/com_echarts/Pie";
export default {
  data() {
    return {
      leftheight: document.documentElement.clientHeight - 30,
      pieOption: null,
      barOption: null
    };
  },
  props: ["lefttown"],
  watch: {
    leftheight: function(val) {
      this.leftheight = val;
    }
  },
  components: {
    combar: combar,
    compie: compie
  },
  mounted() {
    const that = this;
    $("#interval").css(
      "margin-top",
      (document.documentElement.clientHeight * 0.05 - 6) / 2
    );
    $(window).resize(function() {
      that.leftheight = document.documentElement.clientHeight - 30;
      $("#interval").css(
        "margin-top",
        (document.documentElement.clientHeight * 0.05 - 6) / 2
      );
    });
    this.init();
  },
  methods: {
    init() {
        this.setData();
    },
    setData: function() {
      this.setPieData();
      this.setBarData();
    },
    setPieData() {
      this.pieOption = {
        color: ["#20C684", "#E2B72D", "#ED5150", "#FA6AB2", "#434343"],
        tooltip: {
          trigger: "item",
          formatter: "{d}%"
        },
        title: {
          text: "总数",
          subtext: 140,
          textStyle: {
            fontSize: 14,
            color: "#ffffff",
            fontWeight: "normal"
          },
          subtextStyle: {
            color: "#4BDFFF",
            fontSize: 14,
            fontWeight: "bold"
          },
          left: "35%",
          top: "31%"
          //left: '29%',
          //top: '26%'
        },
        legend: {
          orient: "horizontal",
          bottom: 80,
          left: 2,
          selectedMode: false,
          data: ["Ⅱ类", "Ⅲ类", "Ⅳ类", "Ⅴ类", "劣Ⅴ类"],
          width: 240,
          textStyle: {
            color: "#ffffff"
          }
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: ["40%", "78%"],
            center: ["45%", "40%"],
            //radius: ['40%', '78%'],
            //center: ['40%', '35%'],
            label: {
              normal: {
                show: true,
                position: "inner",
                formatter: "{d}%"
              }
            },
            labelLine: {
              normal: {
                show: false
              }
            },
            data: [
              { value: 20, name: "Ⅱ类" },
              { value: 30, name: "Ⅲ类" },
              { value: 30, name: "Ⅳ类" },
              { value: 10, name: "Ⅴ类" },
              { value: 10, name: "劣Ⅴ类" }
            ]
          }
        ]
      };
    },
    setBarData() {
      this.barOption = {
        color: "#50a6ff",
        title: {
          left: "center",
          textStyle: {
            color: "#666666",
            fontWeight: "normal",
            fontSize: 14
          }
        },
        legend: {},
        xAxis: {
          type: "category",
          data: [
            "庄行",
            "拓林",
            "南桥",
            "青村",
            "金汇",
            "奉城",
            "四团",
            "海湾"
          ],
          axisLabel: {
            color: "#ffffff",
            interval: 0, //横轴信息全部显示
            rotate: 30 //-30度角倾斜显示
          },
          axisTick: {
            show: false
          },
          axisLine: {
            show: false
          },
          splitLine: { show: false } //去除网格线
        },
        yAxis: {
          type: "value",
          name: "镇达标率",
          min: 60,
          nameTextStyle: {
            color: "#ffffff",
            padding: 10
          },
          axisLine: {
            show: false
          },
          axisTick: {
            show: false
          },
          axisLabel: {
            textStyle: {
              color: "#ffffff"
            }
          },
          splitLine: {
            show: true, //保留网格线
            lineStyle: {
              color: "#3d4997"
            }
          }
        },
        series: [
          {
            name: "达标率",
            type: "bar",
            barWidth: "60%",
            data: [100, 95, 100, 93, 100, 86, 91, 100],
            label: {
              normal: {
                show: true,
                position: "top",
                formatter: "{c} %",
                color: "#ffffff"
              }
            }
          }
        ]
      };
    },
    getFaterSzdbl: function() {
      this.$emit("getszdbl-event");
    }
  }
};
</script>
<style scoped>
#leftContent {
  width: 100%;
  overflow: hidden;
  min-width: 288px;
  position: relative;
  /*margin-left:8px;*/
}
@media screen and (min-height: 768px) {
  #leftContent .col {
    height: 45px !important;
  }
  #leftContent .centerdiv {
    margin-top: 50px !important;
  }
}
.xzqhrow {
  padding: 10px 40px 0 0;
}
.xzqh {
  width: 64px;
  height: 22px;
  line-height: 22px;
  color: white;
  text-align: center;
  float: left;
  background-color: #0056ce;
}
.titlediv {
  position: absolute;
  top: -10px;
  left: 30px;
  z-index: 1246;
  background-color: #051266;
  width: 100px;
  height: 20px;
  color: #22eff8;
  font-size: 16px;
  font-family: "微软雅黑";
}
.hdgkdiv {
  left: 35px !important;
  width: 70px !important;
}
.szybdiv {
  width: 120px !important;
}

.szdiv {
  width: 210px !important;
}

.topdiv {
  height: 20%;
  position: relative;
  border: 1px solid #2979e5;
  width: 99%;
}

.piediv {
  height: 35%;
  margin-top: 16px;
  position: relative;
  border: 1px solid #2979e5;
  width: 99%;
}

#leftContent .bardiv {
  height: 40%;
  position: absolute;
  bottom: 0px;
  border: 1px solid #2979e5;
  width: 99%;
}

.topleft {
  height: 90%;
}

.bottomleft {
  width: 90%;
  height: 30%;
  border: 1px solid yellow;
}

#leftContent .col {
  height: 30px;
  margin-left: 10px;
  margin-top: 10px !important;
  line-height: 30px;
}

#leftContent .colcss {
  color: #ffffff;
  font-size: 14px;
  font-family: "microsoft yahei";
  float: left;
}
#leftContent .numcss {
  font-size: 16px !important;
  line-height: 20px;
}
.labelcss {
  color: #08e5f5 !important;
  font-size: 16px !important;
  font-family: "microsoft yahei" !important;
  float: left !important;
}

.text-gra {
  float: left;
  font-size: 28px;
  font-weight: bold;
  font-family: Arial;
  color: #ffffff;
  line-height: 21px;
  margin-left: 5px !important;
  background: linear-gradient(#ffffff, #05d2ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.centercol {
  height: 90%;
  width: 100%;
}

.piecss {
  height: 95%;
  width: 55% !important;
  float: left;
}

.centerdiv {
  width: 42%;
  height: 80%;
  float: left;
  color: #ffffff;
  font-size: 14px;
  font-family: "Microsoft YaHei";
  margin-top: 30px;
}

.barcss {
  height: 97%;
  width: 90%;
}

.divcss {
  float: left;
  color: #ffffff !important;
  font-size: 16px !important;
  font-family: "microsoft yahei" !important;
}
.paddingcss {
  padding-top: 15px;
}
</style>


