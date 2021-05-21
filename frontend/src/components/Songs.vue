<template>
  <div class="blue-grey darken-3">
    <div>
      <h1>Songs in playlist</h1>
      <br />
      <ul>
        <li
          @click="playSong(song)"
          v-for="song in songs"
          :key="song.key"
          class="pointer"
        >
          <a> {{ song.title }} - {{ song.artist }} </a>

          <br />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex"

export default {
  name: "Songs",
  data() {
    return {
      addClicked: false,
      name: "",
      playlist: this.$store.getters.getCurrentPlaylist,
    }
  },
  computed: {
    songs() {
      return this.$store.state.allSongs
    },
  },
  async mounted() {
    if (this.$store.state.currentPlaylist === undefined) {
      return
    }
    const data = {
      userId: this.$store.state.user.id,
      id: this.$store.state.currentPlaylist.id,
    }
    await this.getSongs(data)
      .then(() => {
        console.log(this.$store.state.allSongs)
      })
      .catch((err) => {
        console.log(err)
      })
  },
  methods: {
    ...mapActions(["getSongs"]),
    ...mapActions(["addSongs"]),
    ...mapActions(["deleteSong"]),

    playSong(song) {
      this.$store.commit("setIsPlaying", false)
      this.$store.commit("setCurrentSong", {
        id: "",
        title: "",
        artist: "",
        album: "",
      })
      this.$store.commit("setCurrentSong", {
        id: song.key,
        title: song.title,
        artist: song.artist,
        album: song.album,
      })

      this.$store.commit("setIsPlaying", true)
    },
    async remove(data) {
      var r = confirm("Do you want to delete this song?")

      if (r === true) {
        const userId = this.$store.state.user.id
        const params = {
          key: data.key,
          id: data.id,
          userId: userId,
        }

        await this.deleteSong(params).then(() => {})
      } else {
        console.log("We dont delete")
      }
    },
    async update() {
      this.songs = this.$store.state.allSongs
      await this.$nextTick(() => {
        this.songs = this.$store.state.allSongs
      })
    },
    async add() {
      const id = this.$store.state.user.id
      const data = { userId: id, name: this.name }

      await this.addSongs(data).then((response) => {
        this.clicked()
      })
    },
  },
}
</script>

<style scoped>
h1 {
  color: white;
}

li > a {
  color: white;
}
.pointer {
  cursor: pointer;
}
</style>
