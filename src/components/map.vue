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
  props: ['adress'],
  data () {
    return {
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      center: [ this.adress.coord[1], this.adress.coord[0] ],
      zoom: 12,
      markers: [
         {id: 1, imageUrl: 'https://img.icons8.com/ios-filled/50/000000/restaurant.png', coordinates: [ this.adress.coord[1], this.adress.coord[0]]},
        
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