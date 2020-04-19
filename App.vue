<template>
  <app-navigator></app-navigator>
</template>

<script>
import {
  createAppContainer,
  createMaterialTopTabNavigator,
  createStackNavigator
} from "vue-native-router";

import HomeScreen from "./screens/Home.vue";
import CameraScreen from "./screens/Camera.vue";
import WebServiceScreen from "./screens/WebService.vue";
import GeolocationScreen from "./screens/Geolocation.vue";
import DatabaseScreen from "./screens/Database.vue";

const Tabs = createMaterialTopTabNavigator({
  Camera: HomeScreen,
  API: WebServiceScreen,
  Map: GeolocationScreen,
  DB: DatabaseScreen
});

const StackNavigator = createStackNavigator(
  {
    Tabs,
    Camara: CameraScreen
  },
  {
    // initialRouteName: "Home",
    // headerMode: "none"
    // options: {
    //   title: "Prueba",
    // },
    // headerStyle: {
    //   heigh: 20,
    // },
    defaultNavigationOptions: {
      // headerShown: false,
      gestureEnabled: false,
      title: "IISO-4210 - Jose Sandoval",
      headerTitleStyle: {
        textAlign: "center",
        alignSelf: "center",
        color: "white"
      },
      headerStyle: {
        backgroundColor: "#0288D1"
      }
    }
  }
);

const AppNavigator = createAppContainer(StackNavigator);

export default {
  components: { AppNavigator }
};
</script>
