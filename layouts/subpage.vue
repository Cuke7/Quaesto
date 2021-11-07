<template>
  <v-app>
    <v-app-bar app color="primary" class="rounded-b-xl">
      <v-btn icon @click="$router.push('/')">
        <v-icon color="white">
          mdi-arrow-left
        </v-icon>
      </v-btn>

      <v-spacer></v-spacer>

      <v-btn icon @click="share">
        <v-icon color="white">
          mdi-share-variant
        </v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-container>
        <Nuxt></Nuxt>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "subpage",
  data: () => ({
    //
  }),
  methods: {
    share() {
      if (navigator.share) {
        navigator
          .share({
            title: "Quaesto",
            text: "",
            url: window.location.href
          })
          .then(() => console.log("Shared!"))
          .catch(error => console.log("Error sharing", error));
      }
    }
  }
};
</script>
