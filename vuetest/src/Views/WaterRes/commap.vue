<template>
    <div class="szy_center">
        <div id="mapContainer" :style="{height:mapheight+'px'}">
            <div style="position:absolute;top:30px;left:15px;z-index:2000;">
                <div v-on:click="initMapExtent" style="width:24px;height:24px; background:url(/static/Images/swsy/btn_dingw.png) no-repeat;cursor:pointer;"></div>
                <div v-on:click="mapZoomToBig" style="width:24px;height:24px;margin-top:3px;background:url(/static/Images/swsy/btn_jia.png) no-repeat;cursor:pointer;"></div>
                <div v-on:click="mapZoomToSmall" style="width:24px;height:24px;margin-top:2px; background:url(/static/Images/swsy/btn_jian.png) no-repeat;cursor:pointer;"></div>
            </div>
            <el-row class="a-link-water">
                <el-col v-bind:span="24" style="text-align:right;padding-right:20px;">
                    <div v-on:click="aLinkEvent" style="display:inline-block;width:13px;height:13px; background:url(/static/Images/ico_link_white.png) no-repeat;cursor:pointer;"></div>
                    <a v-on:click="aLinkEvent" style="color:#ffffff;">点击跳转自来水公司网站查看供水详情</a>
                </el-col>
            </el-row>
            <div id="show_map">

            </div>
            <div class="map_buttom_content">
                <el-row>
                    <el-col v-bind:span="8" v-bind:offset="2" style="border: 2px solid #0146b2;"></el-col>
                </el-row>
                <el-row>
                    <el-col v-bind:span="22" v-bind:offset="2">
                        图标代表规上企业（月用水量在3000吨以上）
                    </el-col>
                </el-row>
                <el-row>
                    <el-col v-bind:span="22" v-bind:offset="2">
                        红色代表上月企业实际用水量超过计划用水量
                    </el-col>
                </el-row>
                <el-row>
                    <el-col v-bind:span="22" v-bind:offset="2">
                        监测点超标的具体污染物会直接显示在显示点旁
                    </el-col>
                </el-row>
            </div>
        </div>
        <div id="mapTitle">
            {{mapTitle}}
        </div>
        <el-dialog title="详情" v-bind:visible.sync="centerDialogVisible" v-bind:before-close="mapHandleClose" center>
        </el-dialog>
    </div>
</template>
<script>
import $ from "jquery";
export default {
  data() {
    return {
      mapheight: document.documentElement.clientHeight - 32,
      mapTitle: "用水分布",
      map: null,
      centerDialogVisible: false,
      layers: null,
      markerLayer: null, //具体测站图层
      fxbaselayerurl:
        "http://222.66.154.70:8090/iserver/services/map-ugcv5-FengXianJiChuDiTu4/rest/maps/FengXianJiChuDiTu",
      markerPointData: [
        {
          x: 121.37811,
          y: 30.93234,
          twon: "庄行镇",
          statId: "fx-10012",
          type: 1
        },
        {
          x: 121.45077,
          y: 30.95084,
          twon: "南桥镇",
          statId: "fx-10013",
          type: 0
        },
        {
          x: 121.43657,
          y: 30.84199,
          twon: "柘林镇",
          statId: "fx-10014",
          type: 0
        },
        {
          x: 121.55052,
          y: 30.97956,
          twon: "金汇镇",
          statId: "fx-10015",
          type: 0
        },
        {
          x: 121.54104,
          y: 30.91457,
          twon: "青村镇",
          statId: "fx-10016",
          type: 1
        },
        {
          x: 121.60198,
          y: 30.85589,
          twon: "海湾镇",
          statId: "fx-10017",
          type: 0
        },
        {
          x: 121.65426,
          y: 30.93564,
          twon: "奉城镇",
          statId: "fx-10018",
          type: 0
        },
        {
          x: 121.72939,
          y: 30.94165,
          twon: "四团镇",
          statId: "fx-10019",
          type: 0
        }
      ]
    };
  },
  watch: {
    mapheight: function(val) {
      this.mapheight = val;
    }
  },
  mounted() {
    this.initMap();
    const that = this;
    $(window).resize(function() {
      that.mapheight = document.documentElement.clientHeight - 32;
    });
  },
  methods: {
    initMap: function() {
      this.map = new SuperMap.Map("show_map", {
        controls: [
          new SuperMap.Control.ScaleLine(),
          //new SuperMap.Control.MousePosition(),
          //new SuperMap.Control.Zoom(),
          new SuperMap.Control.Navigation({
            dragPanOptions: {
              enableKinetic: true
            }
          })
        ],
        allOverlays: true
      });
      var baselayer = new SuperMap.Layer.TiledDynamicRESTLayer(
        "2016年奉贤基础底图",
        this.fxbaselayerurl,
        null,
        { maxResolution: "auto" }
      );
      //标注点图层
      this.markerLayer = new SuperMap.Layer.Markers("标注");

      this.layers = [baselayer];
      baselayer.events.on({ layerInitialized: this.addLayers });
    },
    //加载图层信息
    addLayers: function() {
      this.map.addLayers(this.layers);
      if (window.screen.width <= 1024) {
        this.map.zoomTo(0);
      } else if (1024 < window.screen.width && window.screen.width <= 1280) {
        this.map.zoomTo(1);
      } else if (1280 < window.screen.width && window.screen.width <= 1366) {
        this.map.zoomTo(1);
      } else if (1366 < window.screen.width && window.screen.width <= 1600) {
        this.map.zoomTo(1);
      } else {
        this.map.zoomTo(2);
      }
      this.map.setCenter(new SuperMap.LonLat(121.56, 30.84));
      this.addMarkerPoints(); //加载marker标注点
    },
    //添加标注点
    addMarkerPoints: function() {
      this.map.addLayer(this.markerLayer);
      var that = this;
      var markers = [];
      $.each(that.markerPointData, function(index, item) {
        var size = new SuperMap.Size(25, 35);
        var offset = new SuperMap.Pixel(-(size.w / 2), -size.h);
        var jpgUrl = "/static/Images/szysy/ico_greenpoint.png";
        switch (item.type) {
          case 1:
            jpgUrl = "/static/Images/szysy/ico_redpoint.png";
            break;
          case 0:
            jpgUrl = "/static/Images/szysy/ico_greenpoint.png";
            break;
        }
        var icon = new SuperMap.Icon(jpgUrl, size, offset);
        var bzmarker = new SuperMap.Marker(
          new SuperMap.LonLat(item.x, item.y),
          icon
        );
        bzmarker.id = item.statId;
        bzmarker.twon = item.twon;
        that.markerLayer.addMarker(bzmarker);
        markers.push(bzmarker);
      });
      for (var i = 0; i < markers.length; i++) {
        markers[i].events.on({ click: this.clickMarkerPoint });
      }
    },
    //点击marker标注点触发的事件
    clickMarkerPoint: function(e) {
      this.centerDialogVisible = true; //弹出模态框（详情信息）
      //加载标注点的详情信息
      this.showDialogFun(e.object);
    },
    //显示详情弹框
    showDialogFun: function(obj) {},
    initMapExtent() {
      if (window.screen.width <= 1024) {
        this.map.zoomTo(0);
      } else if (1024 < window.screen.width && window.screen.width <= 1280) {
        this.map.zoomTo(1);
      } else if (1280 < window.screen.width && window.screen.width <= 1366) {
        this.map.zoomTo(1);
      } else if (1366 < window.screen.width && window.screen.width <= 1600) {
        this.map.zoomTo(1);
      } else {
        this.map.zoomTo(2);
      }
      this.map.setCenter(new SuperMap.LonLat(121.56, 30.84));
    },
    mapZoomToBig() {
      this.map.zoomTo(this.map.getZoom() + 1);
    },
    mapZoomToSmall() {
      this.map.zoomTo(this.map.getZoom() - 1);
    },
    aLinkEvent: function() {
      window.open("http://www.fengxianwater.com/");
    },
    mapHandleClose: function(done) {
      done();
    }
  }
};
</script>
<style scoped>
#mapContainer {
  width: 100%;
  border: 1px solid #0146b2;
  overflow: hidden;
  min-width: 400px;
  /*min-height: 610px;*/
  background-color: #051266;
}

#mapContainer .a-link-water {
  position: absolute;
  right: 10px;
  top: 10px;
  /* margin-bottom: 20px; */
  font-family: "微软雅黑";
  font-size: 14px;
  color: #1ed6e4;
  z-index: 1500;
}

#mapContainer #show_map {
  width: 100%;
  height: 100%;
  min-width: 300px;
  min-height: 400px;
  border: 0px;
  background-color: #051266;
}

#mapContainer .map_buttom_content {
  position: absolute;
  width: 100%;
  height: 140px;
  left: 1px;
  bottom: 1px;
  font-family: "微软雅黑";
  font-size: 13px;
  color: #ffffff;
  z-index: 1500;
  background-color: #051266;
}

.map_buttom_content .el-row {
  margin-top: 7px;
}

.map_buttom_content .el-row:first-child {
  margin-top: 25px;
}

#mapTitle {
  position: absolute;
  top: -10px;
  left: 35px;
  z-index: 1250;
  background-color: #051266;
  width: 100px;
  height: 30px;
  color: #1ed6e4;
  font-family: "微软雅黑";
  text-align: center;
}

.szy_center {
  width: 100%;
  /*height: 100%;
        padding: 10px 0 0 10px;*/
  position: relative;
}
/*比例尺样式重写*/
.smControlScaleLine {
  z-index: 1600 !important;
  width: 100%;
  bottom: 10px !important;
}

.smControlScaleLineTop {
  border: 2px solid #1e50bc !important;
  border-top: none !important;
  text-align: center;
  width: 100px !important;
  position: absolute;
  bottom: 0;
  left: 8%;
  color: white;
}

.smControlScaleLineBottom {
  display: none !important;
}

/**具体测站点的弹框样式*/
.el-dialog {
  height: 484px;
  width: 916px;
  background: url(/static/Images/swsy/img_bg_cezhanbig.png) no-repeat;
  background-size: 1073px 484px;
  position: relative;
}

.el-dialog__header {
  padding: 40px 57px 0px;
}

.el-dialog__title {
  line-height: 24px;
  font-size: 18px;
  font-family: 微软雅黑;
  color: white;
}

.el-dialog__body {
  width: 800px;
  height: 375px;
  margin: -30px auto 0 auto !important;
}

.el-dialog__headerbtn .el-dialog__close {
  margin: 18px 38px 0 0 !important;
}

.el-tabs__item {
  color: white;
}
</style>

