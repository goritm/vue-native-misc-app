<template>
  <view class="container">
    <view v-if="showUsers">
      <button :on-press="generateUsers" title="Generar 5 Usuarios Aleatorios" />
      <view class="test" v-for="(usuario, index) in users" :key="index">
        <touchable-opacity :on-press="() => getPhotos(usuario.user.username)">
          <image class="foticoperfil" :source="{ uri: usuario.user.profile_image.large }" />
          <text class="text-fotos">{{ usuario.user.username }}</text>
        </touchable-opacity>
      </view>
    </view>

    <view v-else-if="showPhotos">
      <button :on-press="changePage" title="Volver a usuarios anteriores" />
      <text class="text-color-primary">{{ clickedUser }}</text>
      <view class="test" v-for="(foto, index) in photos" :key="index">
        <image class="foticos" :source="{ uri: foto.urls.small }" />
        <text class="text-fotos" v-if="foto.description">
          {{
          foto.description
          }}
        </text>
        <text class="text-fotos" v-else-if="foto.alt_description">
          {{
          foto.alt_description
          }}
        </text>
        <text class="text-fotos" v-else>Sin descripción</text>
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
      message: "Límite del API alcanzado",
      users: null,
      photos: null,
      clickedUser: "",
      mostrarUsuarios: true,
      mostrarFotos: false,
      client_id: "0p8wscsQuJqN3zUY6sERifnnK-oYMUf5TLlupMAzb2Y"
    };
  },
  mounted() {
    this.generateUsers();
  },
  methods: {
    generateUsers() {
      axios
        .get(
          "https://api.unsplash.com/photos/random/?count=5&client_id=" +
            this.client_id
        )
        .then(response => {
          this.users = response.data;
          // console.log(this.users);
        })
        .catch(error => {
          console.log(error);
        });
    },
    getPhotos(usuario) {
      this.changePage();
      this.clickedUser = usuario;
      console.log(this.clickedUser);
      axios
        .get(
          "https://api.unsplash.com/users/" +
            this.clickedUser +
            "/photos?per_page=2&client_id=" +
            this.client_id
        )
        .then(response => {
          this.photos = response.data;
          // console.log(this.photos);
        })
        .catch(error => {
          console.log(error);
        });
    },
    changePage() {
      this.mostrarUsuarios = !this.mostrarUsuarios;
      this.mostrarFotos = !this.mostrarFotos;
    }
  },
  computed: {
    showUsers() {
      return this.users && this.mostrarUsuarios;
    },
    showPhotos() {
      return this.photos && this.mostrarFotos;
    }
  }
};
</script>

<style>
.container {
  /* flex: 1; */
  background-color: white;
}
.test {
  align-items: center;
  justify-content: center;
  padding-top: 10;
}
.text-color-primary {
  color: lightseagreen;
  text-decoration-line: underline;
  padding-top: 10;
  font-size: 30;
  text-align: center;
}
.text-fotos {
  color: lightseagreen;
  font-size: 14;
  text-align: center;
}
.foticoperfil {
  width: 105;
  height: 105;
  margin: 3;
  align-items: center;
  justify-content: center;
}
.foticos {
  width: 250;
  height: 250;
  margin: 10;
  align-items: center;
  justify-content: center;
}
</style>
