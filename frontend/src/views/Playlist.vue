<template>
  <v-main>
    <div>
      <h1>Playlist</h1>
      <ul>
        <li v-for="song in content" :key="song.name">
          {{ song.name }}
        </li>
      </ul>

      <!--  -->
    </div>
  </v-main>
</template>

<script>
import { mapState } from "vuex"
export default {
  name: "Playlist",
  mounted() {},
}
</script>
