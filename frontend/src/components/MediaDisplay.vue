<template>
  <v-main class="blue-grey darken-3">
    <div>
      <youtube
        :video-id="videoId"
        ref="youtube"
        :player-vars="playerVars"
        @ready="load()"
        fitParent
        resize
      ></youtube>
    </div>
  </v-main>
</template>

<script>
export default {
  data() {
    return {
      videoId: "",
      playerVars: {
        autoplay: 1,
        controls: 0,
      },
    }
  },
  computed: {
    player() {
      return this.$refs.youtube.player
    },
    video() {
      return this.$store.state.currentSong.id
    },
    play() {
      return this.$store.state.player.isPlaying
    },
    pause() {
      return this.$store.state.player.isPlaying
    },
    volume() {
      return this.$store.state.player.volume
    },
  },
  watch: {
    video(id) {
      this.videoId = id
    },
    play() {
      if (this.$store.state.player.isPlaying === true) {
        this.$refs.youtube.player.playVideo()
      }
    },
    pause() {
      if (this.$store.state.player.isPlaying === false) {
        this.$refs.youtube.player.pauseVideo()
      }
    },
    volume(volume) {
      this.$refs.youtube.player.setVolume(volume)
    },
  },
  methods: {
    load() {
      this.$refs.youtube.player.cueVideoById(this.$store.state.currentSong.id)
      this.$store.commit("setIsPlaying", false)
    },
  },
}
</script>

<style scoped>
body {
  background-color: #292b2c;
  color: white;
}
</style>
