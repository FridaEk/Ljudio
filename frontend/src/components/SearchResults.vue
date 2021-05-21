<template>
  <div class="dropdown pull-right">
    <h1>Showing results for {{ this.$store.state.searchList.search }}</h1>
    <br />
    <br />
    <ul>
      <li
        @click="setSong(song)"
        v-for="song in this.$store.state.searchList.songs"
        :key="song.videoId"
      >
        {{ song.name }}-{{ song.artist.name }}
        <v-btn
          @click="add(song)"
          class="mx-2"
          fab
          light
          small
          color="blue-grey lighten-4"
        >
          <v-icon dark>
            mdi-plus
          </v-icon>
        </v-btn>
      </li>
    </ul>
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex"

export default {
  name: "SearchResults",
  data() {
    return {
      result: this.$store.state.searchList,
      playlist: this.$store.state.currentPlaylist,
    }
  },
  methods: {
    ...mapGetters(["getSearchList"]),
    ...mapActions(["addSong"]),
    setSong(song) {
      this.$store.commit("setIsPlaying", false)
      this.$store.commit("setCurrentSong", {
        id: "",
        title: "",
        artist: "",
        album: "",
      })
      this.$store.commit("setCurrentSong", {
        id: song.videoId,
        title: song.name,
        artist: song.artist.name,
        album: song.album.name,
      })

      this.$store.commit("setIsPlaying", true)
    },
    async add(song) {
      let plist = this.$store.state.currentPlaylist
      if (plist === undefined) return
      const format = {
        id: this.$store.state.currentPlaylist.id,
        key: song.videoId,
        userId: this.$store.state.user.id,
        title: song.name,
        artist: song.artist.name,
        album: song.album.name,
      }
      await this.addSong(format).then((response) => {
        console.log(response)
      })
    },
  },
}
</script>
<style>
#border {
  background: grey;
  border-style: solid;
  margin-right: 90vw;
}
h1 {
  color: white;
}
ul {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  width: 100%;
  padding-left: 0;
}
li {
  color: white;
  padding-bottom: 10%;
  list-style-type: none;
}
button {
  width: 100%;
  height: 70%;
  color: #00cec9;
  top: -15%;
  left: 1%;
}
#buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 3vw;
}
.mx-2 {
  float: right;
}
</style>
