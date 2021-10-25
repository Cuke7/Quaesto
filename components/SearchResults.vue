<template>
  <div>
    <div v-for="(searchResult, index) in formatedSearchResults" :key="index">
      <v-card @click="navigateTo(searchResult)" class="my-12 pa-4">
        <div
          v-for="(ligne, index2) in searchResult.text"
          :key="index2"
          class="my-2"
        >
          <div v-html="highlight(ligne)"></div>
        </div>
      </v-card>
    </div>
  </div>
</template>

<script>
import cec from "/static/cec.js";

export default {
  props: ["searchResults", "searchQuery"],
  data: () => ({
    cec: cec
  }),
  methods: {
    navigateTo(searchResult) {
      console.log(searchResult);
      let id = searchResult.id.join("-");
      this.$router.push("/" + id);
    },
    highlight(text) {
      let matchedTerms = this.searchQuery.replaceAll(" ", "|");
      console.log(matchedTerms);

      if (!this.searchQuery) {
        return text;
      }
      return text.replace(new RegExp("(?:^|(?<= ))(" + matchedTerms + ")(?:(?= )|$)", "gi"), match => {
        return '<span class="highlightText">' + match + "</span>";
      });
    }
  },
  computed: {
    formatedSearchResults() {
      let formatedSearchResults = [];
      if (this.searchResults.length > 0) {
        for (let i = 0; i < 5; i++) {
          let searchResult = this.searchResults[i];

          let id = searchResult.ref.split(",");

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

          console.log(ligne, references.length);

          // console.log(
          //   this.cec[partie][section][chapitre][article][paragraphe][reference]
          // );

          formatedSearchResults.push({
            matchedTerms: Object.keys(searchResult.matchData.metadata),
            id: id,
            text: formatedReferences
          });
        }
      }
      // console.log(formatedSearchResults);
      return formatedSearchResults;
    }
  }
};
</script>
