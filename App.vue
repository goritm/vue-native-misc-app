<template>
  <view class="container">
    <view class="statusbar" />
    <camera class="container" :type="this.type" ref="camera" />
    <view class="meme">
      <touchable-opacity class="suaguiti" :on-press="accessCameraRoll">
        <Ionicons class="iconos" name="ios-photos" />
      </touchable-opacity>

      <touchable-opacity class="suaguiti" :on-press="snapPhoto" v-if="!estaGrabando">
        <FontAwesome class="iconos" name="camera" />
      </touchable-opacity>

      <touchable-opacity class="suaguiti" :on-press="pararVideo" v-if="estaGrabando">
        <FontAwesome class="iconos" name="stop" />
      </touchable-opacity>

      <touchable-opacity class="suaguiti" :on-press="grabarVideo" v-else>
        <Ionicons class="iconos" name="ios-videocam" />
      </touchable-opacity>

      <touchable-opacity class="suaguiti" :on-press="handleCameraType">
        <MaterialCommunityIcons class="iconos" name="camera-switch" />
      </touchable-opacity>
    </view>
  </view>
</template>

<script>
import * as Permissions from "expo-permissions";
import { Camera } from "expo-camera";
import * as ImagePicker from "expo-image-picker";
import {
  FontAwesome,
  Ionicons,
  MaterialCommunityIcons
} from "@expo/vector-icons";
export default {
  data() {
    return {
      estaGrabando: false,
      hasPermission: false,
      type: Camera.Constants.Type.back
    };
  },
  mounted() {
    Permissions.askAsync(
      Permissions.CAMERA,
      Permissions.AUDIO_RECORDING,
      Permissions.CAMERA_ROLL
    )
      .then(status => {
        hasPermission = status.status == "granted" ? true : false;
      })
      .catch(err => {
        console.log(err);
      });
  },
  methods: {
    handleCameraType() {
      // const { cameraType } = this.type;
      alert(type);
      this.type === Camera.Constants.Type.back
        ? Camera.Constants.Type.front
        : Camera.Constants.Type.back;
      alert(type);
    },
    snapPhoto() {
      let photo = this.$refs.camera.takePictureAsync();
      console.log(photo);
    },
    accessCameraRoll() {
      let result = ImagePicker.launchImageLibraryAsync({
        mediaTypes: ImagePicker.MediaTypeOptions.Images
      });
    },
    grabarVideo() {
      let video = this.$refs.camera.recordAsync();
      this.estaGrabando = true;
      console.log(video);
    },
    pararVideo() {
      this.$refs.camera.stopRecording();
      this.estaGrabando = false;
    }
  },
  components: { Camera, FontAwesome, Ionicons, MaterialCommunityIcons }
};
</script>

<style>
.statusbar {
  background-color: black;
  height: 100;
}
.container {
  flex: 1;
  background-color: black;
}
.meme {
  flex-direction: row;
  justify-content: space-between;
  margin: 60px 20px;
}
.suaguiti {
  align-self: flex-end;
  align-items: center;
  background-color: transparent;
}
.iconos {
  color: white;
  font-size: 50;
}
</style>
