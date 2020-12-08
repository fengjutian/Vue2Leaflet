<template>
<div class='vue-leaflet'>
  <l-map style="width: 100%; height: 100vh;" :zoom="zoom" :center="center">
    <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
    <l-marker :lat-lng="marker">
      <l-popup :content="text"></l-popup>
    </l-marker>
  </l-map>

  <div id='map'></div>

</div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup } from 'vue2-leaflet'
import L from 'leaflet'
export default {
  name: 'VueLeaflet',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup
  },
  data () {
    return {
      zoom: 10,
      center: L.latLng(76.40,119.93),
     // layerType: 'vec',

     //  url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
     url: 'http://t0.tianditu.com/vec_c/wmts?layer=vec&style=default&tilematrixset=c&Service=WMTS&Request=GetTile&Version=1.0.0&Format=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}&tk=f0c94499ef7f7f653c3aced1e2d5b7f9',

      attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      marker: L.latLng(76.40,119.93),
      text: 'this is a marker',
      zoomOffset: 1
    }
  },
  mounted() {

    // var url='http://t0.tianditu.com/vec_c/wmts?layer=vec&style=default&tilematrixset=c&Service=WMTS&Request=GetTile&Version=1.0.0&Format=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}&tk=f0c94499ef7f7f653c3aced1e2d5b7f9';
    // var emap = new L.TileLayer.WMTS( url ,
    //   {
    //       tileSize:256,
    //       layer: 'vec',
    //       style: "default",
    //       tilematrixSet: "c",
    //       format: "tile",
    //       attribution: "<a href='https://github.com/mylen/leaflet.TileLayer.WMTS'>GitHub</a>&copy; <a href='http://www.ign.fr'>IGN</a>"
    //   }
    // );
    // var map = L.map('map',{crs:L.CRS.EPSG4326,center: {lon:112 , lat:40},zoom: 13})
    // map.addLayer(emap)





   }
 }
</script>

<style scoped>

</style>
