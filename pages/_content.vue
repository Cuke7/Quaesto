<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="px-4">
        <div v-for="(ligne, index) in lignes" :key="index" class="my-4">
          <div v-if="index == 0" class="text-body-1 font-weight-bold">
            {{ ligne }}
          </div>
          <div v-else class="text-body-1">
            {{ ligne }}
          </div>
        </div>
      </div>
    </v-col>
  </v-row>
</template>

<script>
import cec from "/static/cec.js";
export default {
  layout: "subpage",

  async asyncData({ params }) {
    const content = params.content; // When calling /abc the slug will be "abc"
    return { content };
  },

  data: () => ({
    cec: cec
  }),

  computed: {
    lignes() {
      let id = this.content.split("-");
      let partie = Number(id[0]);
      let section = Number(id[1]);
      let chapitre = Number(id[2]);
      let article = Number(id[3]);
      let paragraphe = Number(id[4]);
      let reference = Number(id[5]);

      return this.cec[partie][section][chapitre][article][paragraphe][
        reference
      ];
    }
  }
};
</script>
