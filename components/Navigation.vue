<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-chip color="primary" outlined v-bind="attrs" v-on="on">
          Navigation
        </v-chip>
      </template>
      <v-card>
        <v-toolbar dark color="primary">
          <v-btn icon @click="dialog = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>Table des matières</v-toolbar-title>
        </v-toolbar>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      dialog: false
    };
  }
};
</script>
