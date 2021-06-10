<template>
  <v-app>
    <v-app-bar app color="primary_bg" dark dense>
      <div class="d-flex align-center">
        <v-toolbar-title>Tunes.ME</v-toolbar-title>
      </div>

      <v-spacer></v-spacer>

      <v-btn icon>
        <router-link to="/" custom v-slot="{ navigate }">
          <span
            @click="navigate"
            @keypress.enter="navigate"
            role="link"
            tag="icon"
          >
            <v-icon>mdi-home</v-icon>
          </span>
        </router-link>
      </v-btn>

      <v-btn icon>
        <router-link to="/songs" custom v-slot="{ navigate }">
          <span
            @click="navigate"
            @keypress.enter="navigate"
            role="link"
            tag="icon"
          >
            <v-icon>mdi-playlist-music</v-icon>
          </span>
        </router-link>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>
<script>
export default {
  name: "App",
  components: {},
  data: () => ({
    links: ["Home", "About Us", "Team", "Services", "Blog", "Contact Us"],
  }),
};
</script>
