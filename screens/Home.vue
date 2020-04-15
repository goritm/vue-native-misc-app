<template>
  <view class="container">
    <view class="statusbar" />

    <view class="efe">
      <button title="Tomar Foto" @press="tomarFoto"></button>
      <image class="test" :source="{ uri: fotomeme }" />
    </view>

    <view class="efe">
      <button title="Tomar Video" @press="tomarVideo"></button>
      <video class="test" :source="{ uri: videomeme }" useNativeControls resizeMode="contain" />
    </view>
  </view>
</template>

<script>
import { Video } from "expo-av";
export default {
  data() {
    return {
      fotomeme:
        "https://miro.medium.com/max/1800/1*jHgVL8ln-1-P8CGcvUck0g.jpeg",
      videomeme:
        "https://cdn.discordapp.com/attachments/691871512247926815/695174141174874132/spiderman_3.mp4"
    };
  },
  props: {
    navigation: {
      type: Object
    }
  },
  methods: {
    tomarFoto() {
      this.navigation.navigate("Camara", {
        onGoBack: this.refresh,
        tipo: "Foto"
      });
    },
    tomarVideo() {
      this.navigation.navigate("Camara", {
        onGoBack: this.refresh,
        tipo: "Video"
      });
    },
    refresh(data, tipo) {
      if (tipo == "Foto") {
        this.fotomeme = data;
      } else {
        this.videomeme = data;
      }
    }
  },
  components: { Video }
};
</script>

<style>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
}
.statusbar {
  background-color: white;
  /* height: 110; */
  height: 20;
}
.efe {
  padding: 10;
}
.test {
  width: 300;
  height: 300;
}
</style>
