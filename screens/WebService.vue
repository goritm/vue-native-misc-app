<template>
  <view class="container">
    <text class="text-color-primary">{{message}}</text>
  </view>
</template>

<script>
export default {
  data: function() {
    return {
      message: "Aquí irá la vaina del web service lol"
    };
  }
};
</script>

<style>
.container {
  /* display: flex; */
  flex: 1;
  background-color: white;
  /* align-items: center; */
  justify-content: center;
}
.text-color-primary {
  color: purple;
  font-size: 30;
  text-align: center;
}
</style>