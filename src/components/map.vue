<template>
  <l-map
    :center="center"
    :zoom="zoom"
    class="map"
    ref="map"
    @update:zoom="zoomUpdated"
    @update:center="centerUpdated"
  >
    <l-tile-layer
      :url="url"
    >
    </l-tile-layer>
    <Restau
      v-for="marker in markers"
      :key="marker.id"
      :marker="marker"
    >
    </Restau>
  </l-map>
</template>

<script>
import { LMap, LTileLayer } from 'vue2-leaflet';
import Restau from './restau'

import 'leaflet/dist/leaflet.css'
export default {
  components: {
    LMap,
    LTileLayer,
    Restau
  },
  data () {
    return {
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      center: [ 40.75637123,-73.98545321 ],
      zoom: 12,
      markers: [
         {id: 1, imageUrl: 'https://img.icons8.com/ios-filled/50/000000/restaurant.png', coordinates: [ 40.7676919, -73.98513559999999]},
        
      ]
    }
  },
  methods: {
    zoomUpdated (zoom) {
      this.zoom = zoom;
    },
    centerUpdated (center) {
      this.center = center;
    },

  }
}
</script>

<style>
  .map {
    position: absolute;
    width: 100%;
    height: 100%;
    overflow :hidden
  }
</style>