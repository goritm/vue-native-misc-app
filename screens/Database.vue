<template>
  <view class="container">
    <text class="text-color-primary">{{ message }}</text>
  </view>
</template>

<script>
import * as SQLite from "expo-sqlite";

// const db = SQLite.openDatabase("db");

// db.exec([], false);

// https://github.com/react-native-community/async-storage
// https://medium.com/swlh/react-native-with-sqlite-1ec64702e35e
// https://docs.expo.io/versions/latest/sdk/sqlite/

export default {
  data() {
    return {
      message: "Hello World",
    };
  },
};
</script>

<style>
.container {
  flex: 1;
  background-color: white;
  align-items: center;
  justify-content: center;
}
.text-color-primary {
  color: blue;
  font-size: 30;
}
</style>
