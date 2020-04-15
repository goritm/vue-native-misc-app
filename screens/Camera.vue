<template>
  <view class="container">
    <view class="statusbar"></view>
    <camera class="container" :type="cameraType" ratio="4:3" ref="camera" />
    <view class="meme">
      <touchable-opacity class="suaguiti" :on-press="accessCameraRoll">
        <ionicons class="iconos" name="ios-photos" />
      </touchable-opacity>

      <touchable-opacity
        class="suaguiti"
        :on-press="snapPhoto"
        v-if="tipoCamara == 'Foto'"
      >
        <fontAwesome class="iconos" name="camera" />
      </touchable-opacity>

      <touchable-opacity
        class="suaguiti"
        :on-press="stopVideo"
        v-if="isRecording"
      >
        <fontAwesome class="iconos" name="stop" />
      </touchable-opacity>

      <touchable-opacity
        class="suaguiti"
        :on-press="recordVideo"
        v-else-if="tipoCamara == 'Video'"
      >
        <ionicons class="iconos" name="ios-videocam" />
      </touchable-opacity>

      <touchable-opacity class="suaguiti" :on-press="handleCameraType">
        <materialCommunityIcons class="iconos" name="camera-switch" />
      </touchable-opacity>
    </view>
  </view>
</template>

<script>
import * as Permissions from "expo-permissions";
import { Camera } from "expo-camera";
import * as ImagePicker from "expo-image-picker";
import * as MediaLibrary from "expo-media-library";
import {
  FontAwesome,
  Ionicons,
  MaterialCommunityIcons,
} from "@expo/vector-icons";
export default {
  data() {
    return {
      isRecording: false,
      hasPermission: false,
      tipoCamara: "",
      cameraType: "back",
    };
  },
  async mounted() {
    this.tipoCamara = this.navigation.state.params.tipo;

    await Permissions.askAsync(
      Permissions.CAMERA,
      Permissions.AUDIO_RECORDING,
      Permissions.CAMERA_ROLL
    )
      .then((status) => {
        this.hasPermission = status.status == "granted" ? true : false;
      })
      .catch((err) => {
        console.log(err);
      });
  },
  props: {
    navigation: {
      type: Object,
    },
  },
  methods: {
    handleCameraType() {
      this.cameraType === "back"
        ? (this.cameraType = "front")
        : (this.cameraType = "back");
    },
    async snapPhoto() {
      let photo = await this.$refs.camera.takePictureAsync();
      // MediaLibrary.saveToLibraryAsync(photo.uri);
      console.log(photo);
      this.navigation.state.params.onGoBack(photo.uri, "Foto");
      this.navigation.goBack();
    },
    async accessCameraRoll() {
      let result = await ImagePicker.launchImageLibraryAsync({
        mediaTypes: ImagePicker.MediaTypeOptions.All,
      });
    },
    async recordVideo() {
      this.isRecording = true;
      let video = await this.$refs.camera.recordAsync();
      // MediaLibrary.saveToLibraryAsync(video.uri);
      console.log(video);
      this.navigation.state.params.onGoBack(video.uri, "Video");
      this.navigation.goBack();
    },
    stopVideo() {
      this.$refs.camera.stopRecording();
      this.isRecording = false;
    },
  },
  components: { Camera, FontAwesome, Ionicons, MaterialCommunityIcons },
};
</script>

<style>
.statusbar {
  background-color: black;
  height: 50;
}
.container {
  flex: 1;
  background-color: black;
}
.meme {
  flex-direction: row;
  justify-content: space-between;
  margin: 70;
  margin-right: 30;
  margin-left: 30;
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
