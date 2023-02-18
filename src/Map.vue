<template>
  <div id="container"></div>
</template>
<script setup>
// 引入地图 JSAPI 的加载器
import AMapLoader from '@amap/amap-jsapi-loader'
import { onMounted, shallowRef } from 'vue'

//安全密钥
window._AMapSecurityConfig = {
  securityJsCode: '8dbd05dea5a22b100c63bf60916488c4',
}

//geoserver发布的图层地址
const BASIC_URL = 'http://39.174.88.209:10886/geoserver/uav/wms'

//图层名称列表
const layers = [
  'uav:D',
  'uav:G区已剪裁',
  'uav:G区未剪裁',
  'uav:H区',
  'uav:I区',
  'uav:测试合并',
  'uav:测试合并1',
  'uav:测试航线速度快',
  'uav:测试航线速度正常',
]

//地图实例
let map = shallowRef(null)

onMounted(() => {
  //初始化
  initMap()
})

//初始化地图
const initMap = () => {
  AMapLoader.load({
    key: '9688df702bc89ec33ea2ef93d14a75d4', // 申请好的Web端开发者Key，首次调用 load 时必填
    version: '2.0', // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
    plugins: [], // 需要使用的的插件列表，如比例尺'AMap.Scale','AMap.ToolBar', 'AMap.Driving'等
  })
    .then(AMap => {
      map = new AMap.Map('container', {
        //设置地图容器id
        viewMode: '2D', //是否为3D地图模式
        zoom: 20, //初始化地图级别
        center: [103.30530434372851, 23.452173035115045], //初始化地图中心点位置 中国 - [105.602725, 37.076636]
      })

      // 加载插件
      //   AMap.plugin('AMap.ToolBar', function () {
      //     //异步加载插件
      //     const toolbar = new AMap.ToolBar()
      //     map.addControl(toolbar)
      //   })

      //添加第三方图层
      addLayerToMap()
    })
    .catch(e => {
      console.log(e)
    })
}

//添加第三方图层
const addLayerToMap = () => {
  layers.forEach(layer => {
    //创建wms标准图层
    const wms = new AMap.TileLayer.WMS({
      url: BASIC_URL, // wms服务的url地址
      blend: false, // 地图级别切换时，不同级别的图片是否进行混合
      tileSize: 512, // 加载WMS图层服务时，图片的分片大小
      zIndex: 0,
      params: {
        LAYERS: layer,
        VERSION: '1.1.0',
        //   TRANSPARENT: 'TRUE',
        FORMAT: 'image/png',
      }, // OGC标准的WMS地图服务的GetMap接口的参数
    })
    //将图层添加到地图中
    wms.setMap(map)
  })
}
</script>
<style scoped>
#container {
  padding: 0px;
  margin: 0px;
  width: 100%;
  height: 100%;
}
</style>
