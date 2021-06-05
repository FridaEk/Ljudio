<template>
  <div class="main">
    <div>
      <h1>Playlists</h1>
      <br />
      <ul>
        <li
          @click="selectPlaylist(playlist)"
          v-for="playlist in playlists"
          :key="playlist.id"
          class="pointer"
        >
          {{ playlist.name }}
          <v-btn
            @click="remove(playlist.id)"
            class="mx-2"
            fab
            light
            small
            color="blue-grey lighten-4"
          >
            <v-icon dark>
              mdi-minus
            </v-icon>
          </v-btn>
        </li>
      </ul>
    </div>
    <div v-if="!this.addClicked">
      <v-btn
        rounded
        small
        color="blue-grey lighten-4"
        ligth
        @click="clicked"
        max-width="120"
      >
        Add playlist
      </v-btn>
    </div>
    <div v-else>
      <v-text-field
        @submit.prevent="name"
        v-model="name"
        dense
        flat
        rounded
        dark
        solo-inverted
        placeholder="Playlist name"
      >
      </v-text-field>
      <v-btn
        rounded
        small
        color="blue-grey lighten-4"
        ligth
        @click="add"
        max-width="120"
      >
        Add
      </v-btn>
      <v-btn
        rounded
        small
        color="blue-grey lighten-4"
        ligth
        @click="clicked"
        max-width="120"
      >
        Cancel</v-btn
      >
    </div>
  </div>
</template>

<script>
import { mapActions, mapMutations } from "vuex"

export default {
  name: "Playlist",
  computed: {},
  data() {
    return {
      addClicked: false,
      name: "",
    }
  },
  computed: {
    playlists() {
      return this.$store.getters.getAllPlaylists
    },
    playlistSelect(playlist) {
      if (playlist.id == this.selected) {
        return `Selected: ${playlist.name}`
      } else {
        return playlist.name
      }
    },
  },
  async mounted() {
    await this.getPlaylists(this.$store.state.user.id)
      .then((response) => {
        console.log(response)
      })
      .catch((err) => {
        console.log(err)
      })
  },
  methods: {
    ...mapActions(["getPlaylists"]),
    ...mapActions(["addPlaylist"]),
    ...mapActions(["deletePlaylist"]),
    ...mapActions(["getSongs"]),
    ...mapMutations(["setCurrentPlaylist"]),
    async clicked() {
      this.addClicked = !this.addClicked
      await this.$nextTick()
    },
    async add() {
      const id = this.$store.state.user.id
      const data = {
        userId: id,
        name: this.name,
      }

      await this.addPlaylist(data).then(() => {
        this.addClicked = !this.addClicked
        this.name = ""
      })
    },
    async remove(id) {
      var r = confirm("Do you want to delete this playlist?")

      if (r === true) {
        const userId = this.$store.state.user.id
        const data = {
          userId: userId,
          id: id,
        }

        await this.deletePlaylist(data).then(() => {})
      } else {
        console.log("We dont delete")
      }
    },
    async selectPlaylist(playlist) {
      const wrap = {
        id: playlist.id,
        name: playlist.name,
        userId: playlist.user_id,
      }

      this.setCurrentPlaylist(wrap)
      delete wrap.name
      this.selected = playlist.id
      await this.getSongs(wrap).then(() => {
        this.$store.state.allSongs
      })
    },
  },
}
</script>

<style>
.btn {
  color: white;
}

.pointer {
  cursor: pointer;
}
</style>
