<template>
  <div class="daiski__player" @mousemove="toggleControls">
    <video
      class="viewer"
      src="../assets/videos/video.mp4"
      @canplay="updatePaused"
      @playing="updatePaused"
      @pause="updatePaused"
      @timeupdate="handlerUpdate"
      @click="togglePlay"
      @dblclick="toggleFullscreen"
    ></video>
    <div class="controls">
      <div class="progress">
        <input
          type="range"
          class="progress__control"
          step="0.0001"
          min="0"
          @input="videoElement.currentTime = currentTime"
          :max="videoElement !== null ? videoElement.duration : 0"
          v-model="currentTime"
        />
        <div class="progress__filled" :style="{ width: timeLine }"></div>
      </div>
      <a @click="togglePlay" class="player__button">
        <i v-show="paused == true" class="daiski-play"></i>
        <i v-show="paused == false" class="daiski-pause"></i>
      </a>
      <input
        type="range"
        name="volume"
        class="player__volume"
        min="0"
        max="1"
        step="0.05"
        v-model="volume"
        @input="videoElement.volume = volume"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      videoElement: null,
      paused: null,
      currentTime: 0,
      canScrub: false,
      volume: 1,
      timeout: null
    };
  },
  methods: {
    updatePaused(event) {
      this.videoElement = event.target;
      this.paused = event.target.paused;
    },
    togglePlay() {
      const method = this.paused ? "play" : "pause";
      this.videoElement[method]();
    },
    handlerUpdate() {
      console.log("ok");
      this.currentTime = this.videoElement.currentTime;
    },
    toggleFullscreen() {
      const player = document.querySelector(".daiski__player");
      if (!document.fullscreenElement) {
        player.requestFullscreen().catch(err => {
          alert(
            `Error attempting to enable full-screen mode: ${err.message} (${err.name})`
          );
        });
      } else {
        document.exitFullscreen();
      }
    },
    toggleControls() {
      const player = document.querySelector(".daiski__player");
      const controls = document.querySelector(".controls");
      controls.classList.add("show");
      player.classList.add("show");
      if (this.timeout !== null) {
        window.clearTimeout(this.timeout);
      }
      this.timeout = window.setTimeout(function() {
        controls.classList.remove("show");
        player.classList.remove("show");
      }, 2000);
    }
  },
  computed: {
    playing() {
      return !this.paused;
    },
    timeLine() {
      if (this.videoElement !== null) {
        const percent = (this.currentTime / this.videoElement.duration) * 100;
        return percent + "%";
      } else {
        return "0%";
      }
    }
  }
};
</script>

<style scoped>
@import "../assets/css/player.css";
@import "../assets/icons/css/daiski.css";
</style>
