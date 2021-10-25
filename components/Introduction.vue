<template>
  <v-dialog v-model="dialog" transition="dialog-top-transition">
    <template v-slot:activator="{ on, attrs }">
      <v-chip color="primary" outlined v-bind="attrs" v-on="on">
        Introduction
      </v-chip>
    </template>
    <v-card>
      <v-card-title>
        Introduction
      </v-card-title>
      <v-card-text class="pb-0">
        <div
          v-for="(ligne, index) in intro"
          :key="index"
          class="my-2 text-justify"
        >
          <div class="text-body-1">
            {{ ligne }}
          </div>
        </div>
      </v-card-text>
      <v-card-actions class="pa-4">
        <v-spacer> </v-spacer>
        <v-btn color="primary" outlined @click="dialog = false">
          Fermer
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ["intro"],
  data() {
    return {
      dialog: false
    };
  }
};
</script>
