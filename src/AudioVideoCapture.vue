<template>
  <div id="audio-video-capture">
    <div v-show="videoStatus">
      <video ref="video" id="video" width="640" height="480" autoplay></video>
    </div>
    <div>
      <button v-on:click="joinConference()">Join conf</button>
      <template v-if="videoStatus">
        <button v-on:click="stopConference()">Stop conf</button>
      </template>
    </div>
  </div>
</template>

<script>
  export default {
    name: "AudioVideoCapture",
    data() {
      return {
        video: {},
        videoStatus: false
      }
    },
    methods: {
      joinConference() {
        this.videoStatus = true;
        if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
          navigator.mediaDevices.getUserMedia({video: true}).then(stream => {
            this.video.srcObject = stream;
            this.video.play();
          });
        }
      },
      stopConference() {
        let track = this.video.srcObject.getTracks()[0];
        track.stop();
        this.videoStatus = false;
      }
    },
    mounted() {
      this.video = this.$refs.video;
    },
  }
</script>

<style scoped>
  #audio-video-capture {
    font-family: Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin-top: 60px;
  }
</style>
