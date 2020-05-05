<template>
  <div>
    <h1>{{ msg }}</h1>
    <div class="row">
      <div class="col-md-6">
          <web-cam 
            ref="webcam"
            :device-id="deviceId"
            width="100%"
            @started="onStarted"
            @stopped="onStopped"
            @error="onError"
            @cameras="onCameras"
            @camera-change="onCameraChange"/>
      </div>

       <div class="col-md-6">

         <img :src="img" alt="" width="100%" v-if="img !== null">
         <img src="@/assets/logo.png" alt="" height="400px" v-else>
       </div>

       
    </div>
   
      <button type="button" @click="onCapture" class="btn btn-primary">Capture photo</button>
     
  </div>
</template>

<script>
import { WebCam } from "vue-web-cam";

export default {
  name: 'MyWebCam',
  components: {
    WebCam
  },
  data() {
      return {
          msg: 'VueWebCam',
          img: null,
            camera: null,
            deviceId: null,
            devices: []
      }
    },
   watch: {
        camera: function(id) {
            this.deviceId = id;
        },
        devices: function() {
            // Once we have a list select the first one
            const [first] = this.devices;
            if (first) {
                this.camera = first.deviceId;
                this.deviceId = first.deviceId;
            }
        }
    },
    methods: {
        onCapture() {
            this.img = this.$refs.webcam.capture();
        },
        onStarted(stream) {
            console.log("On Started Event", stream);
        },
        onStopped(stream) {
            console.log("On Stopped Event", stream);
        },
        onStop() {
            this.$refs.webcam.stop();
        },
        onStart() {
            this.$refs.webcam.start();
        },
        onError(error) {
            console.log("On Error Event", error);
        },
        onCameras(cameras) {
            this.devices = cameras;
            console.log("On Cameras Event", cameras);
        },
        onCameraChange(deviceId) {
            this.deviceId = deviceId;
            this.camera = deviceId;
            console.log("On Camera Change Event", deviceId);
        }
    }
}
</script>

