<template>
    <div id="rightContent" v-bind:style="{height:rightheight+'px'}">
        <div class="righttop">
            <div class="topdiv">
                <div class="imgcss"><img src="/static/Images/szysy/ico_yongsxl.png"></div>
                <el-row type="flex" align="middle" class="toprow">
                    <el-col v-bind:span="10">
                        <span class="fontcss">奉贤区总供水量：</span>
                    </el-col>
                    <el-col v-bind:span="5">
                        <span class="numcss">400万m³</span>
                    </el-col>
                </el-row>
                <el-row type="flex" align="middle" class="toprow2">
                    <el-col v-bind:span="10">
                        <span class="fontcss">奉贤区总供水量：</span>
                    </el-col>
                    <el-col v-bind:span="5">
                        <span class="numcss">400万m³</span>
                    </el-col>
                </el-row>
                <el-row type="flex" align="middle" class="toprow3">
                    <el-col v-bind:span="6">
                        <span class="fontcss">用水效率:</span>
                    </el-col>
                    <el-col v-bind:span="4">
                        <span class="numcss">80.2%</span>
                    </el-col>
                    <el-col v-bind:span="8">
                        <span class="fontcss">对比去年上升:</span>
                    </el-col>
                    <el-col v-bind:span="5">
                        <span class="numcss">10.2%</span>
                        <span><img src="/static/Images/szysy/ico_zengz.png"></span>
                    </el-col>
                </el-row>
            </div>

            <div class="righttopdiv">
                <div class="bgcolorcss lsdiv">
                    <div class="fontcss aligncss leftpadding" style="padding-top: 10px;">漏损水量</div>
                    <div class="numcss aligncss leftpadding">80万m³</div>
                    <div class="numcss aligncss leftpadding">9.5% <span><img src="/static/Images/szysy/ico_zengz.png"></span></div>
                </div>

                <el-row class="bgcolorcss rightdiv">
                    <el-col class="fontcss aligncss" style="padding-bottom: 12px;">
                        <span class="spancss">免费供水</span>
                        <span class="spancss">12万m³</span>
                        <span class="spancss">10.23% <span><img src="/static/Images/szysy/ico_zengz.png"></span></span>
                    </el-col>
                </el-row>
                <el-row class="bgcolorcss rightdiv">
                    <el-col class="fontcss aligncss" style="padding-bottom: 12px;">
                        <span class="spancss">物理漏水</span>
                        <span class="spancss">12万m³</span>
                        <span class="spancss">10.23% <span><img src="/static/Images/szysy/ico_zengz.png"></span></span>
                    </el-col>
                </el-row>
                <el-row class="bgcolorcss rightdiv">
                    <el-col class="fontcss aligncss" style="padding-bottom: 12px;">
                        <span class="spancss">账面漏水</span>
                        <span class="spancss">12万m³</span>
                        <span class="spancss">10.23% <span><img src="/static/Images/szysy/ico_zengz.png"></span></span>
                    </el-col>
                </el-row>
                <el-row class="bgcolorcss rightdiv">
                    <el-col class="fontcss aligncss" style="padding-bottom: 12px;">
                        <span class="spancss">其他漏水</span>
                        <span class="spancss">12万m³</span>
                        <span class="spancss">10.23% <span><img src="/static/Images/szysy/ico_xiaj.png"></span></span>
                    </el-col>
                </el-row>
                <div>
                </div>
            </div>
        </div>
        <div class="rightbottom" id="interval">
            <div class="title">用水户管理统计</div>
            <el-row type="flex" align="middle" justify="center" class="bottomrow1">
                <el-col v-bind:span="7"><div class="fontcss">用水户总数(户):</div></el-col>
                <el-col v-bind:span="4"><div class="numcss ">12345</div></el-col>
                <el-col v-bind:span="6"><div class="fontcss">水表总数(户):</div></el-col>
                <el-col v-bind:span="7"><div class="numcss">3343</div></el-col>
            </el-row>

            <el-row type="flex" align="middle" justify="center" class="bottomrow2">
                <el-col v-bind:span="2"></el-col>
                <el-col v-bind:span="7">
                    <span><img src="/static/Images/szysy/ico_kaihu.png"></span>
                </el-col>
                <el-col v-bind:span="7">
                    <span><img src="/static/Images/szysy/ico_xiaohu.png"></span>
                </el-col>
                <el-col v-bind:span="7">
                    <span><img src="/static/Images/szysy/ico_fenhu.png"></span>
                </el-col>
                <el-col v-bind:span="1"></el-col>
            </el-row>
            <el-row type="flex" align="middle" justify="center" class="bottomrow3">
                <el-col v-bind:span="8" style="text-align: center;">
                    <span class="fontcss">开户:</span><span class="numcss "> 13422</span>
                </el-col>
                <el-col v-bind:span="8">
                    <span class="fontcss">销户:</span><span class="numcss "> 22534</span>
                </el-col>
                <el-col v-bind:span="8">
                    <span class="fontcss">分户:</span><span class="numcss "> 41234</span>
                </el-col>
            </el-row>

            <div class="tablediv">
                <el-row class="headercss titlecss">
                    <el-col v-bind:span="3">序号</el-col>
                    <el-col v-bind:span="6">用水户名称</el-col>
                    <el-col v-bind:span="6">超出计划用水量(m³)</el-col>
                    <el-col v-bind:span="5">未交费用水量(m³)</el-col>
                </el-row>
                <div class="tablecss">
                    <el-row class="titlecss" v-for="li in lists" v-bind:key="li.id">
                        <el-col v-bind:span="3" style="text-align:center;">{{li.id}}</el-col>
                        <el-col v-bind:span="9" v-bind:title="li.name">{{retListName(li.name)}}</el-col>
                        <el-col v-bind:span="5">{{li.exceWater}}</el-col>
                        <el-col v-bind:span="6">{{li.noAmountWater}}</el-col>
                    </el-row>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import $ from "jquery";
export default {
  data() {
    return {
      rightheight: document.documentElement.clientHeight - 32,
      lists: [
        {
          id: "1",
          name: "巴斯夫聚氨酯有限公司",
          exceWater: "12344",
          noAmountWater: "12344"
        },
        {
          id: "2",
          name: "巴斯夫聚氨酯有限公司ww",
          exceWater: "12344",
          noAmountWater: "12344"
        },
        {
          id: "3",
          name: "巴斯夫聚氨酯有限公司哈哈",
          exceWater: "12344",
          noAmountWater: "12344"
        },
        {
          id: "4",
          name: "巴斯夫聚氨酯有限公司版本",
          exceWater: "12344",
          noAmountWater: "12344"
        },
        {
          id: "5",
          name: "巴斯夫聚氨酯有限公司密码",
          exceWater: "12344",
          noAmountWater: "12344"
        },
        {
          id: "6",
          name: "巴斯夫聚氨酯有限公司",
          exceWater: "12344",
          noAmountWater: "12344"
        }
      ]
    };
  },
  watch: {
    rightheight: function(val) {
      this.rightheight = val;
    }
  },
  mounted() {
    const that = this;
    $(window).resize(function() {
      that.rightheight = document.body.clientHeight - 32;
    });
  },
  methods: {
    retListName: function(objname) {
      var objname = objname + "";
      if (objname.length > 7) {
        return objname.substring(0, 7) + "...";
      } else {
        return objname;
      }
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
      var contentH = docH - 30;
      // this.styles.height = (contentH / 3) + 'px';
      this.height = contentH / 2;
    }
  }
};
</script>
<style scoped>
.bottomrow1 {
  margin: 15px 0 0 30px;
  width: 100%;
}
.bottomrow2 {
  padding-top: 10px;
  padding-left: 25px;
}

.bottomrow3 {
  padding-top: 10px;
  padding-left: 25px;
  width: 90%;
}

.tablediv {
  width: 100%;
  height: 72%;
  bottom: 0;
  position: absolute;
}
#rightContent {
  width: 100%;
  overflow: hidden;
  min-width: 350px;
  min-height: 500px;
  position: relative;
  padding-right: 5px;
}

.righttop {
  height: 38%;
  width: 100%;
  border: 1px solid #2979e5;
}

.rightbottom {
  height: 59%;
  width: 100%;
  border: 1px solid #2979e5;
  position: absolute;
  bottom: 0;
  margin-top: 16px;
}

.imgcss {
  float: left;
  /*margin-top: 20px;*/
  margin-left: 30px;
  height: 45px;
}

.headercss {
  background-color: #2d3a8d;
  margin-top: 14px;
  text-align: center !important;
  height: 45px !important;
}

.toprow {
  margin-top: 18px;
  padding-left: 10px;
}
.toprow2 {
  padding-left: 10px;
}
#rightContent .topdiv {
  margin-top: 50px;
}
.toprow3 {
  left: -38px;
  top: 10px;
}
.ysxl {
  width: 180px !important;
  text-align: center;
  left: 45px !important;
}

.righttopdiv {
  margin: 22px 0 0 15px;
}

.rightdiv {
  margin: 0 0 10px 15px;
  float: left;
  text-align: center;
  height: 20px;
  line-height: 20px;
  width: 60%;
}

.lsdiv {
  width: 110px;
  float: left;
}

.leftpadding {
  padding-bottom: 16px;
}

.fontcss {
  color: #ffffff;
  font-size: 14px;
  font-family: "microsoft yahei";
  text-align: left;
}

.aligncss {
  text-align: center !important;
}

.spancss {
  padding-left: 8px;
}

.bgcolorcss {
  background-color: #2d3a8d;
}

.numcss {
  color: #ffffff;
  font-size: 14px;
  font-family: Arial;
  font-weight: bold;
  text-align: left;
  margin-left: 0;
}
.tablecss::-webkit-scrollbar {
  display: none;
}
.tablecss {
  width: 100%;
  height: 72%;
  margin-top: 10px;
  overflow: auto;
}

/*#rightContent.tablecss .titlecss:nth-child(2){
        margin-top:15px;
    }*/

.titlecss {
  text-align: left;
  color: #ffffff;
  float: left;
  font-size: 14px;
  font-family: "Microsoft YaHei";
  height: 30px;
  padding-top: 8px;
  width: 100%;
}

.title {
  position: absolute;
  top: -10px;
  left: 35px;
  z-index: 1400;
  background-color: #051266;
  width: 120px;
  height: 20px;
  color: #22eff8;
  font-size: 16px;
  font-family: "microsoft yahei";
}

@media screen and (min-width: 1280px) {
  .titlecss {
    height: 45px !important;
  }
}
@media screen and (max-width: 1024px) {
  .imgcss {
    float: left;
    margin-top: 8px !important;
    margin-left: 30px;
    height: 40px !important;
  }
  .toprow {
    margin-top: 5px !important;
    padding-left: 10px !important;
  }

  .toprow2 {
    padding-left: 10px !important;
  }

  .toprow3 {
    left: -48px !important;
    top: 5px !important;
  }

  .bottomrow1 {
    margin: 10px 0 0 5px !important;
  }

  .bottomrow2 {
    padding-top: 5px !important;
    padding-left: 5px !important;
  }

  .bottomrow3 {
    padding-top: 0px !important;
    padding-left: 0px !important;
    width: 90% !important;
  }

  .lsdiv {
    width: 75px !important;
  }

  .righttopdiv {
    margin: 12px 0 0 4px !important;
  }

  .rightdiv {
    margin: 0 0 10px 5px !important;
  }
  .tablecss {
    width: 100% !important;
    height: 65% !important;
    margin-top: 10px !important;
  }
  .headercss {
    padding-top: 8px !important;
  }
  #rightContent {
    width: 100% !important;
    overflow: hidden !important;
    min-width: 350px !important;
    position: relative !important;
  }
}
@media screen and (max-height: 768px) {
  #rightContent .topdiv {
    margin-top: 0px !important;
  }

  .toprow {
    margin-top: 10px !important;
  }

  .toprow3 {
    top: 0;
  }

  .righttopdiv {
    margin: 10px 0 0 15px;
  }
}
</style>

