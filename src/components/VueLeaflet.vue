<template>
<div class='vue-leaflet'>
  <l-map style="width: 100%; height: 100vh;" :zoom="zoom" :center="center">
    <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
    <l-marker :lat-lng="marker">
      <l-popup :content="text"></l-popup>
    </l-marker>
  </l-map>

</div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup } from 'vue2-leaflet'
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
      zoom: 13,
      center: L.latLng(47.413220, -1.219482),
      // url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      url: 'http://api.tianditu.gov.cn/api?v=4.0&tk=a3db440b1e1b8f00eeeae7961e75caff&lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}',

      //  http://api.tianditu.gov.cn/api?v=4.0&tk=a3db440b1e1b8f00eeeae7961e75caff
      // http://webrd01.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}

      attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      marker: L.latLng(47.413220, -1.219482),
      text: 'this is a marker'

    }
  }
}
</script>

<style scoped>

</style>
