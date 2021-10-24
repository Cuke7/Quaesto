<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-text-field
        class="mt-6 mx-4"
        v-model="searchQuery"
        append-icon="mdi-magnify"
        @click:append="search"
        @keydown.enter="search"
        label="Rechercher dans le catéchisme de l'église catholique"
        color="primary"
        hide-details
        autocomplete="off"
      ></v-text-field>
      <div class="px-4">
        <SearchResults
          :searchResults="searchResults"
          :searchQuery="searchQuery"
        ></SearchResults>
      </div>
      <v-snackbar v-model="snackbar" timeout="1000">
        Recherche en cours...
      </v-snackbar>
    </v-col>
  </v-row>
</template>

<script>
import cec from "/static/cec.js";

import lunr from "lunr";

require("lunr-languages/lunr.stemmer.support")(lunr);
require("lunr-languages/lunr.fr")(lunr);

export default {
  mounted() {
    let documents = [];

    // Build Lunr index
    for (let p = 0; p < cec.length; p++) {
      let partie = cec[p];
      // console.log(partie);
      for (let s = 2; s < partie.length; s++) {
        let section = partie[s];
        // console.log(section);
        for (let c = 2; c < section.length; c++) {
          let chapitre = section[c];
          // console.log(chapitre);
          for (let a = 2; a < chapitre.length; a++) {
            let article = chapitre[a];
            // console.log(article);
            for (let para = 2; para < article.length; para++) {
              let paragraphe = article[para];
              // console.log(paragraphe);
              for (let r = 2; r < paragraphe.length; r++) {
                let reference = paragraphe[r];
                // console.log(reference);

                // let lignes = reference.reduce((a, b) => {
                //   return a + " " + b;
                // });

                for (let l = 0; l < reference.length; l++) {
                  let ligne = reference[l];
                  let id = [p, s, c, a, para, r, l];
                  documents.push({
                    id: id,
                    text: ligne
                  });
                }
              }
            }
          }
        }
      }
    }

    let idx = lunr(function() {
      this.use(lunr.fr);
      this.ref("id");
      this.field("text");
      // this.metadataWhitelist = ["position"];

      documents.forEach(function(doc) {
        this.add(doc);
      }, this);
    });
    this.idx = idx;
  },
  data: () => ({
    cec: cec,
    searchQuery: "Immaculée conception",
    searchResults: [],
    snackbar: false,
    idx: null
  }),
  methods: {
    search() {
      this.searchResults = this.idx.search(
        this.searchQuery
          .split(" ")
          .join("~1")
          .trim()
      );
    }
  },
  computed: {
    // idx() {
    //   return idx;
    // }
  }
};
</script>

<style>
.highlightText {
  background: yellow;
}
</style>
