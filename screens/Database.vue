<template>
  <view>
    <view class="container">
      <text-input class="text-input" v-model="text" />
      <touchable-opacity class="button" :on-press="addDog">
        <text class="button-text">Agregar</text>
      </touchable-opacity>
      <touchable-opacity class="button" :on-press="clearList">
        <text class="button-text">Reset</text>
      </touchable-opacity>
    </view>
    <view class="test" v-for="(dogo, index) in dogos" :key="index">
      <touchable-opacity :on-press="() => removeDog(dogo)">
        <text class="text-container">{{ dogo }}</text>
      </touchable-opacity>
    </view>
  </view>
</template>

<script>
import { AsyncStorage } from "react-native";

export default {
  data() {
    return {
      text: "",
      dogos: [],
    };
  },
  mounted() {
    this.getDog();
  },
  methods: {
    async addDog() {
      try {
        await AsyncStorage.setItem(this.text, this.text);
        alert(this.text + " agregado!");
        this.dogos.push(this.text);
        this.text = "";
      } catch (error) {
        console.log(error.message);
      }
    },
    async getDog() {
      try {
        const keys = await AsyncStorage.getAllKeys();
        // const items = await AsyncStorage.multiGet(keys);
        this.dogos = keys;
        console.log(this.dogos);
      } catch (error) {
        console.log(error.message);
      }
    },
    async removeDog(dogo) {
      try {
        await AsyncStorage.removeItem(dogo);
        this.dogos = [];
        this.getDog();
      } catch (error) {
        console.log(error.message);
      }
    },
    async clearList() {
      try {
        await AsyncStorage.clear();
        this.dogos = [];
        alert("Datos borrados");
      } catch (error) {
        console.log(error.message);
      }
    },
  },
};
</script>

<style>
.container {
  background-color: #333;
  flex-direction: row;
  padding: 20px;
}
.test {
  align-items: center;
  justify-content: center;
  padding-top: 10;
}
.text-input {
  background-color: white;
  margin-right: 5px;
  flex: 3;
}
.text-container {
  color: black;
  padding: 2;
}
.button {
  flex: 1;
  background-color: #008080;
  margin-left: 5px;
  align-items: center;
  justify-content: center;
  padding-top: 10px;
  padding-bottom: 10px;
}
.button-text {
  color: white;
  font-weight: normal;
}
</style>
