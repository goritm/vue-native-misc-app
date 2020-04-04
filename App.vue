<template>
  <app-navigator></app-navigator>
</template>

<script>
import { createAppContainer, createStackNavigator } from "vue-native-router";

import HomeScreen from "./screens/Home.vue";
import CamaraScreen from "./screens/Camara.vue";

const StackNavigator = createStackNavigator(
  {
    Home: HomeScreen,
    Camara: CamaraScreen,
  },
  {
    initialRouteName: "Home",
    // headerTitle: "ME PICA",
    headerMode: "none",
    // options: {
    //   title: "Prueba",
    // },
    // headerStyle: {
    //   heigh: 20,
    // },
    // defaultNavigationOptions: {
    //   headerShown: false,
    //   gestureEnabled: false,
    // },
  }
);

const AppNavigator = createAppContainer(StackNavigator);

export default {
  components: { AppNavigator },
};
</script>
