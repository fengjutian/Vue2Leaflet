<template>
<div class='vue-leaflet'>
  <!-- <l-map style="width: 100%; height: 100vh;" :zoom="zoom" :center="center">
    <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
    <l-marker :lat-lng="marker">
      <l-popup :content="text"></l-popup>
    </l-marker>
  </l-map> -->

  <div id='map' style='width:100vw;height:100vh;' ></div>

</div>
</template>

<script>
import proj4 from 'proj4'
// import { LMap, LTileLayer, LMarker, LPopup } from 'vue2-leaflet'
import L from 'leaflet'
export default {
  name: 'VueLeaflet',
  components: {
    // LMap,
    // LTileLayer,
    // LMarker,
    // LPopup
  },
  data () {
    return {
      // zoom: 10,
      // center: L.latLng(76.40, 119.93),
      // // layerType: 'vec',

      // //  url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      // // url: 'http://t0.tianditu.gov.cn/img_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={x}&TILECOL={y}&tk=f0c94499ef7f7f653c3aced1e2d5b7f9',
      // url: 'http://t0.tianditu.com/vec_c/wmts?layer=vec&style=default&tilematrixset=c&Service=WMTS&Request=GetTile&Version=1.0.0&Format=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}&tk=f0c94499ef7f7f653c3aced1e2d5b7f9',

      // attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      // marker: L.latLng(76.40, 119.93),
      // text: 'this is a marker',
      // zoomOffset: 1

      map: null,
      point: []
    }
  },
  mounted () {
    this.$nextTick(() => {
      // this.initMap()
    //  this.point = [32.045330, 118.789670]
      this.point = [31.778890, 119.969700]
      this.initLeaflet()

      console.log(1, this.Mector2Wgs84(118.789670, 32.045330))
    })
  },
  methods: {
    initMap () {
      let map = L.map('map', {
        minZoom: 3,
        maxZoom: 14,
        // center: [32.05493, 118.796252],
        center: [76.45, 118.81],
        zoom: 12,
        zoomControl: true,
        attributionControl: false,
        crs: L.CRS.EPSG8357

      })
      window.map = map
      L.tileLayer(
        'http://t0.tianditu.com/vec_c/wmts?layer=vec&style=default&tilematrixset=c&Service=WMTS&Request=GetTile&Version=1.0.0&Format=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}&tk=f0c94499ef7f7f653c3aced1e2d5b7f9'
      ).addTo(map)
    },
    initLeaflet () {
      // 风场样式引用
      var esriDarkGreyCanvas = L.tileLayer(
        'http://t0.tianditu.com/vec_c/wmts?layer=vec&style=default&tilematrixset=c&Service=WMTS&Request=GetTile&Version=1.0.0&Format=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}&tk=f0c94499ef7f7f653c3aced1e2d5b7f9'
      )

      // 生成风场实例
      var velocityLayer = L.velocityLayer({
        displayValues: true,
        displayOptions: {
          velocityType: 'GBR Wind',
          displayPosition: 'bottomleft',
          displayEmptyString: 'No wind data'
        },
        data: require('./wind-global.json'), // 风场数据
        minVelocity: 0, // Velocity：速率
        maxVelocity: 10,
        velocityScale: 0.005,
        particleMultiplier: 1 / 1200, // 粒子的数量
        lineWidth: 5, // 粒子的粗细
        frameRate: 15, // 定义每秒执行的次数
        colorScale: ['rgb(47,112,47)', 'rgb(47,112,47)', 'rgb(47,112,47)', 'rgb(47,112,47)', 'rgb(47,112,47)']
      })

      L.CRS.CustomEPSG4326 = L.extend({}, L.CRS.Earth, {
        code: 'EPSG:4326',
        projection: L.Projection.LonLat,
        transformation: new L.Transformation(1 / 180, 1, -1 / 180, 0.5),
        scale: function (zoom) {
          return 256 * Math.pow(2, zoom - 1)
        }
      })

      // 添加风场样式至地图中
      this.map = L.map('map', {
        // center: [76.45, 118.81],
        center: this.point,
        // center: [31.56430025788347, 119.61018574979072],
        crs: L.CRS.CustomEPSG4326,
        // crs: L.CRS.EPSG3857,
        //  crs: L.CRS.EPSG4326,
        zoom: 12,
        layers: [esriDarkGreyCanvas]
      })

      console.log('getCode', this.map)
      // 风场实例添加到地图上
      velocityLayer.addTo(this.map)
    },
    Mector2Wgs84 (lng, lat) {
      return proj4(proj4('EPSG:4326'), proj4('EPSG:3857'), [lng, lat])
    }

  }
}
</script>

<style scoped>

</style>
