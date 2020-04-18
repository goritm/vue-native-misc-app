<template>
  <view class="container">
    <text>Latitud: {{ coordinates.latitude }}</text>
    <text>Longitud: {{ coordinates.longitude }}</text>
    <touchable-opacity>
      <button title="Obtener Localización" @press="getLocation"></button>
    </touchable-opacity>
    <MapView
      class="mapa"
      :region="coordinates"
      :onRegionChangeComplete="onRegionChange"
      ref="mapa"
    />
  </view>
</template>

<script>
import * as Permissions from "expo-permissions";
import * as Location from "expo-location";
import MapView from "react-native-maps";

// https://github.com/react-native-community/react-native-maps
// https://github.com/react-native-community/react-native-maps/blob/master/docs/mapview.md

export default {
  data() {
    return {
      errorMessage: "",
      coordinates: {
        latitude: 41.881832,
        longitude: -87.623177,
        latitudeDelta: 0.1,
        longitudeDelta: 0.05,
      },
    };
  },
  methods: {
    getLocation() {
      Permissions.askAsync(Permissions.LOCATION)
        .then((status) => {
          if (status !== "granted") {
            this.errorMessage = "Permission to access location was denied";
          }

          Location.getCurrentPositionAsync({}).then((location) => {
            // this.coordinates.latitude = location.coords.latitude;
            // this.coordinates.longitude = location.coords.longitude;
            console.log(this.coordinates);
            this.$refs.mapa.animateToRegion(
              {
                latitude: location.coords.latitude,
                longitude: location.coords.longitude,
                latitudeDelta: 0.001,
                longitudeDelta: 0.05,
              },
              50
            );
            // console.log(this.location.coords.latitude);
          });
        })
        .catch((err) => {
          console.log(err);
        });
    },
    onRegionChange(region) {
      this.coordinates = region;
    },
  },
  components: { MapView },
};
</script>

<style>
.container {
  flex: 1;
  background-color: white;
  align-items: center;
  justify-content: center;
}
.mapa {
  height: 500px;
  width: 400px;
}
.text-color-primary {
  color: blue;
}
</style>
