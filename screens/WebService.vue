<template>
  <view class="container">
    <button :on-press="generarFotos" title="Generar fotos XD" />
    <view v-if="info">
      <view v-for="(foto, index) in info" :key="index">
        <image class="foticos" :source="{ uri: foto.urls.small }" />
        <text class="text-fotos">Tomada por: {{ foto.user.name }}</text>
        <!-- <text class="text-fotos">Nombre: {{ foto.location.name }}</text> -->
      </view>
    </view>
    <view v-else>
      <text class="text-color-primary">{{ message }}</text>
    </view>
  </view>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      message: "Dale al botón XD!",
      info: null,
      client_id: "0p8wscsQuJqN3zUY6sERifnnK-oYMUf5TLlupMAzb2Y"
    };
  },
  methods: {
    generarFotos() {
      axios
        .get(
          "https://api.unsplash.com/photos/random/?count=2&client_id=" +
            this.client_id
        )
        .then(response => {
          this.info = response.data;
          console.log(this.info);
        })
        .catch(error => {
          console.log(error);
          this.message = "Límite de imágenes XD!";
        });
    }
  }
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
  color: lightseagreen;
  font-size: 20;
  text-align: center;
}
.text-fotos {
  color: lightseagreen;
  font-size: 14;
  text-align: center;
}
.foticos {
  width: 300;
  height: 300;
  margin: 10;
}
</style>
