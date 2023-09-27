<template>
  <div id="mapContainer" :style="mapStyle"></div>
</template>

<script>
export default {
  data() {
    return { 
      mapStyle: {
        width: '100%',
        height: '100vh'
      },
      map: null
    };
  },
  mounted() {                
    const script = document.createElement('script');
    script.type = 'text/javascript';
    script.src = `http://api.map.baidu.com/api?v=3.0&ak=XZeDqHavRDPW4TntAiAsUrjz2DRLq3bs&callback=initMap`;

    window.initMap = () => {
      this.initializeMap();
    };

    document.head.appendChild(script);
  },
  methods: {
    initializeMap() {
      const BMap = window.BMap;                                                                                               

      this.map = new BMap.Map('mapContainer');

      const point = new BMap.Point(113.297292, 23.178829);
      this.map.centerAndZoom(point, 16);
      this.map.setMapStyleV2({     
        styleId: 'c7b61f5ede7a6020625bae8517a9b8f1'
      });
      this.map.addControl(new BMap.MapTypeControl());
      this.map.setCurrentCity('北京');

      // 创建自定义覆盖物123456
      const overlayPoint = new BMap.Point(113.297292, 23.178829); // 覆盖物所在的坐标点
      const overlayContent = '<img src="http://101.34.3.13:3000/image/ljt.png" width="32" height="32">'; // 修改为你的图片 URL
      
      const customOverlay = new BMap.Overlay();
      
      customOverlay.initialize = function(map) {
        this._map = map;
        const div = this._div = document.createElement('div');
        div.innerHTML = overlayContent;
        div.style.position = 'absolute';
        div.style.zIndex = BMap.Overlay.getZIndex(overlayPoint.lat);
        div.style.backgroundColor = 'transparent';
        map.getPanes().labelPane.appendChild(div);
        return div;
      };

      customOverlay.draw = function() {
        const map = this._map;
        const pixel = map.pointToOverlayPixel(overlayPoint);
        this._div.style.left = pixel.x + 'px';
        this._div.style.top = pixel.y + 'px';
      };
      
      this.map.addOverlay(customOverlay);
      
      // 其他地图相关操作...
    }
  }
};
</script>

<style>
#mapContainer {
  width: 100%;
  height: 100vh;
}
</style>
