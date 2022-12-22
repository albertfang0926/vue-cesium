<template>
  <div id="container" class="box">
    <div id="cesiumContainer"></div>
  </div>
</template>
 
<script>
import * as Cesium from 'cesium/Cesium'
const baseMap = {
  'arcgisMap' : new Cesium.ArcGisMapServerImageryProvider({
      url: 'http://map.geoq.cn/arcgis/rest/services/ChinaOnlineStreetPurplishBlue/MapServer'        
  }),
  'googleMap': new Cesium.UrlTemplateImageryProvider({
      url: 'http://www.google.cn/maps/vt?lyrs=s@716&x={x}&y={y}&z={z}'        
  }),
  'gebcoMap': new Cesium.WebMapServiceImageryProvider({
    url:'https://www.gebco.net/data_and_products/gebco_web_services/web_map_service/mapserv?',
    layers : '0',
    proxy: new Cesium.DefaultProxy('/proxy/')
  }),
  'tianditu': new Cesium.WebMapTileServiceImageryProvider({		 
    url: 'http://t0.tianditu.gov.cn/img_w/wmts?tk=ebf64362215c081f8317203220f133eb',
    layer:'img',
    style:'default',
    tileMatrixSetID:'w',
    format:'tiles',
    maximumLevel: 18
	})
}

export default {
  name: 'Home',
  data() {
    return {
      viewer: null
    }
  },

  mounted(){
    this.init()
    this.home()
  },

  methods: {
    init() {
      let viewer = new Cesium.Viewer('cesiumContainer',{
          geocoder: false, //搜索框
          homeButton: false, //home按钮
          sceneModePicker: false, //3d/2d 模式切换按钮
          baseLayerPicker: false, //图层选择按钮
          navigationHelpButton: false, //右上角的帮助按钮
          animation: false, //左下角的动画控件的显示
          shouldAnimate: false, //控制模型动画
          timeline: false, //底部的时间轴
          fullscreenButton: false, //右下角的全屏按钮
          selectionIndicator: true, //选择指示器
          infoBox: true, //信息面板
      });
      this.viewer = viewer
      viewer._cesiumWidget._creditContainer.style.display = "none";// 隐藏版权
      this.viewer.imageryLayers.addImageryProvider(baseMap.googleMap)
    },
    home() {
      this.viewer.camera.setView({
        destination : Cesium.Cartesian3.fromDegrees(116, 20, 2e6)
      });
    }
  }
};
</script>
 
<style lang='scss' scoped>
#cesiumContainer {
  width: 100%;
  height: 100%;
}
.box {
  width: 100%;
  height: 100%;
} 
</style>