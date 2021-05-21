<template>
  <v-app id="inspire">
    <v-app-bar app clipped-center flat height="72" class="blue-grey darken-3">
      <v-spacer></v-spacer>
      <!-- Navbar with searchbar -->
      <v-responsive max-width="400">
        <v-text-field
          v-model="query"
          dense
          flat
          rounded
          background-color="blue-grey lighten-4"
          color="black"
        ></v-text-field>
      </v-responsive>
      <v-btn
        id="btn-search"
        rounded
        small
        color="blue-grey lighten-4"
        dark
        @click="search(query)"
        max-width="120"
        >Search
      </v-btn>
      <h1>{{ selected }}</h1>
    </v-app-bar>

    <!-- List with playlists  -->
    <v-navigation-drawer class="blue-grey darken-3" app width="300">
      <Songs />
    </v-navigation-drawer>

    <!-- List with songs -->
    <v-navigation-drawer class="blue-grey darken-3" app clipped right>
      <Register />
      <br />
      <div v-if="this.$store.state.user.loggedIn">
        <Playlist />
      </div>
    </v-navigation-drawer>
    <!-- navbar for mobilescreen -->

    <media-display></media-display>
    <v-main class="blue-grey darken-3">
      <v-card class="blue-grey darken-3" id="search-list">
        <span><SearchResults /></span></v-card
    ></v-main>
    <v-footer app color="transparent" height="140" inset fixed>
      <v-app-bar class="blue-grey darken-3" rounded inset>
        <v-btn
          v-if="!this.$store.state.player.isPlaying"
          color="blue-grey lighten-4"
          width="20%"
          @click="play(true)"
          rounded
        >
          <v-icon>mdi-play</v-icon>
        </v-btn>
        <v-btn
          v-else
          color="blue-grey lighten-4"
          rounded
          width="20%"
          @click="play(false)"
        >
          <v-icon>mdi-pause</v-icon>
        </v-btn>
        <v-slider
          min="0"
          max="100"
          value="100"
          color="dark-gray"
          thumb-label
          append-icon="mdi-volume-high"
          @drag="changeVolume()"
          v-model="value"
          @input="changeVolume(value)"
        >
        </v-slider>
      </v-app-bar>
    </v-footer>
  </v-app>
</template>

<script>
import SearchResults from "./components/SearchResults.vue"
import Playlist from "./components/Playlist.vue"
import { mapActions, mapGetters } from "vuex"
import MediaDisplay from "./components/MediaDisplay.vue"
import Register from "./components/Register.vue"
import Songs from "./components/Songs.vue"

export default {
  name: "App",
  components: {
    MediaDisplay,
    SearchResults,
    Playlist,
    MediaDisplay,
    Register,
    Songs,
  },
  data() {
    return {
      value: 1,
      text: "",
      query: "",
      song: "",
      selected: null,

      value: "100",
    }
  },
  methods: {
    async update() {
      this.selected = this.$store.state.currentPlaylist
      await this.$nextTick(() => {
        this.selected = this.$store.state.currentPlaylist
      })
    },
  },
  mounted() {
    this.$store.dispatch("getUser")
  },
  computed: {
    getSong() {
      return this.$store.state.currentSong.title
    },

    ...mapGetters(["getSearchList"]),
    ...mapGetters(["getCurrentPlaylist"]),
  },
  watch: {
    getSong(song) {
      this.song = song
    },
  },
  methods: {
    ...mapActions(["searchSong"]),

    changeVolume(volume) {
      this.$store.commit("setVolume", volume)
    },
    play(bool) {
      this.$store.commit("setIsPlaying", bool)
    },
    search(query) {
      this.searchSong(query)
    },
  },
}
</script>

<style scoped>
#btn-search {
  color: black;
}
#search-list {
  margin-right: 0.3vw;
}
</style>
