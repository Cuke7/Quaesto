<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-text-field
        v-model="searchQuery"
        append-icon="mdi-magnify"
        @click:append="search"
        @keydown.enter="search"
        label="Rechercher dans le catéchisme de l'église catholique"
        color="primary"
        hide-details
        autocomplete="off"
      ></v-text-field>

      <v-card
        v-for="(searchResult, index) in formatedSearchResults"
        :key="index"
        class="my-12 pa-4"
      >
        <div
          v-for="(ligne, index2) in searchResult.text"
          :key="index2"
          class="my-2"
        >
          <div v-html="highlight(ligne, searchResult)"></div>
        </div>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import cec from "/static/cec.js";

import lunr from "lunr";

require("lunr-languages/lunr.stemmer.support")(lunr);
require("lunr-languages/lunr.fr")(lunr);

export default {
  mounted() {},
  data: () => ({
    cec: cec,
    searchQuery: "",
    searchResults: []
  }),
  methods: {
    highlight(text, searchResults) {
      let matchedTerms = searchResults.matchedTerms.join(" ").replace(" ", "|");

      if (!this.searchQuery) {
        return text;
      }
      console.log(matchedTerms);
      return text.replace(new RegExp("(" + matchedTerms + ")", "gi"), match => {
        return '<span class="highlightText">' + match + "</span>";
      });
    },
    search() {
      this.id = null;
      this.searchResults = this.idx.search(
        this.searchQuery.split(" ").join("~1")
      );
      console.log(this.searchQuery.split(" ").join("~1"));
    }
  },
  computed: {
    indexes() {
      return this.id.split("");
    },
    idx() {
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

                  let lignes = reference.reduce((a, b) => {
                    return a + " " + b;
                  });

                  for (let l = 0; l < reference.length; l++) {
                    let ligne = reference[l];
                    documents.push({
                      id:
                        String(p) +
                        String(s) +
                        String(c) +
                        String(a) +
                        String(para) +
                        String(r) +
                        String(l),
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
      return idx;
    },
    formatedSearchResults() {
      let formatedSearchResults = [];
      if (this.searchResults.length > 0) {
        for (let i = 0; i < 5; i++) {
          let searchResult = this.searchResults[i];

          let id = searchResult.ref.split("");

          let partie = Number(id[0]);
          let section = Number(id[1]);
          let chapitre = Number(id[2]);
          let article = Number(id[3]);
          let paragraphe = Number(id[4]);
          let reference = Number(id[5]);
          let ligne = Number(id[6]);

          let references = this.cec[partie][section][chapitre][article][
            paragraphe
          ][reference];

          let formatedReferences = [];

          if (ligne > 0) {
            formatedReferences.push(references[ligne - 1]);
          }
          formatedReferences.push(references[ligne]);
          if (ligne < references.length - 1) {
            formatedReferences.push(references[ligne + 1]);
          }

          formatedSearchResults.push({
            matchedTerms: Object.keys(searchResult.matchData.metadata),
            id: String(searchResult.ref),
            text: formatedReferences
          });
        }
      }
      console.log(formatedSearchResults);
      return formatedSearchResults;
    }
  }
};
</script>

<style>
.highlightText {
  background: yellow;
}
</style>
