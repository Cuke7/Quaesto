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
    </v-col>
  </v-row>
</template>

<script>
export default {
  mounted() {
    //
  },
  data: () => ({
    searchQuery: "immaculée conception",
    searchResults: []
  }),
  methods: {
    search() {
      console.log(this.idx);

      this.searchResults = this.idx.search(this.searchQuery.trim());
    }
  },
  computed: {
    cec() {
      return this.$store.state.cec;
    },
    idx() {
      return this.$store.getters.idx;
    }
  }
};
</script>

<style>
.highlightText {
  background: yellow;
}
</style>
