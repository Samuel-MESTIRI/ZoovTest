<template>
  <div id="map-wrap" style="height: 100vh">
    <no-ssr>
      <l-map :zoom=12 :center="[2.3464418, 48.7277591]">
        <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
        <l-marker v-for="(bike, i) in bikes" :key="i" :lat-lng="bike.location.coordinates">
          <l-icon
            :id="bike['_id']"
            :icon-size="[48, 48]"
            :icon-anchor="[32, 32]"
            :icon-url="require('~/assets/icons/bike.png')" >
          </l-icon>
          <l-popup>
            <BikeDetails :bike="bike" />
          </l-popup>
        </l-marker>
      </l-map>
    </no-ssr>
  </div>
</template>

<script>
  import BikeDetails from '~/components/BikeDetails.Vue'

  export default {
    components: {
      BikeDetails
    },
    props: {
      bikes: {
        type: Array,
        default: []
      }
    }
  }
</script>

