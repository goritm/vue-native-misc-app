<template>
  <view class="container">
    <view class="statusbar" />
    <camera class="container" :type="cameraType" ratio="4:3" ref="camera" />
    <view class="meme">
      <touchable-opacity class="suaguiti" :on-press="accessCameraRoll">
        <Ionicons class="iconos" name="ios-photos" />
      </touchable-opacity>

      <touchable-opacity
        class="suaguiti"
        :on-press="snapPhoto"
        v-if="!isRecording"
      >
        <FontAwesome class="iconos" name="camera" />
      </touchable-opacity>

      <touchable-opacity
        class="suaguiti"
        :on-press="stopVideo"
        v-if="isRecording"
      >
        <FontAwesome class="iconos" name="stop" />
      </touchable-opacity>

      <touchable-opacity class="suaguiti" :on-press="recordVideo" v-else>
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
import * as MediaLibrary from "expo-media-library";
import {
  FontAwesome,
  Ionicons,
  MaterialCommunityIcons
} from "@expo/vector-icons";
export default {
  data() {
    return {
      isRecording: false,
      hasPermission: false,
      cameraType: "back"
    };
  },
  async mounted() {
    await Permissions.askAsync(
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
      this.cameraType === "back"
        ? (this.cameraType = "front")
        : (this.cameraType = "back");
    },
    async snapPhoto() {
      let photo = await this.$refs.camera.takePictureAsync();
      MediaLibrary.saveToLibraryAsync(photo.uri);
      console.log(photo);
      // const { uri } = await this.$refs.camera.takePictureAsync();
      // console.log(uri);
      // const asset = await MediaLibrary.createAssetAsync(uri);
      // console.log(asset);
      // MediaLibrary.createAlbumAsync("GoriXD", asset);
    },
    async accessCameraRoll() {
      let result = await ImagePicker.launchImageLibraryAsync({
        mediaTypes: ImagePicker.MediaTypeOptions.All
      });
    },
    async recordVideo() {
      this.isRecording = true;
      let video = await this.$refs.camera.recordAsync();
      MediaLibrary.saveToLibraryAsync(video.uri);
      console.log(video);
    },
    stopVideo() {
      this.$refs.camera.stopRecording();
      this.isRecording = false;
    }
  },
  components: { Camera, FontAwesome, Ionicons, MaterialCommunityIcons }
};
</script>

<style>
.statusbar {
  background-color: black;
  height: 110;
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
