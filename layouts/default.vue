<template>
  <v-app>
    <v-app-bar app color="primary">
      <v-toolbar-title>Quaesto</v-toolbar-title>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    //
  })
};
</script>

<style>
.v-main {
  background-image: url("/background.jpg");
  background-repeat: repeat;
}

/* .v-app-bar {
  background-image: url("/background.jpg");
  background-repeat: repeat;
}*/

.v-card {
  background-image: url("/background.jpg");
  background-repeat: repeat;
} 
</style>
