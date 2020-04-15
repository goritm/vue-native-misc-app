<template>
  <view class="container">
    <text>Location:</text>
    <text>{{ location }}</text>
    <touchable-opacity>
      <button title="Obtener Localización" @press="getLocation"></button>
    </touchable-opacity>
    <!-- <mapview class="mapa" /> -->
  </view>
</template>

<script>
import * as Permissions from "expo-permissions";
import * as Location from "expo-location";
// import MapView from "react-native-maps";
// import { Constants } from "expo";

// https://vue-native.io/docs/community-libraries-doc.html
// https://vue-native.io/docs/device-apis.html
// https://github.com/react-native-mapbox-gl/maps/blob/master/docs/GettingStarted.md

export default {
  data() {
    return {
      location: {},
      errorMessage: "",
      coordinates: {
        latitude: 12.91074,
        longitude: 77.5996363,
        latitudeDelta: 0.0922,
        longitudeDelta: 0.0421,
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

          Location.getCurrentPositionAsync({}).then((location1) => {
            this.location = location1;
            console.log(this.location.coords);
            // alert(this.location.coords.accuracy);
          });
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  //   components: { MapView },
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
  flex: 1;
}
.text-color-primary {
  color: blue;
}
</style>
