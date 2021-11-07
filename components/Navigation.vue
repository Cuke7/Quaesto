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
        <v-app-bar dark color="primary">
          <v-btn icon @click="dialog = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>Table des matières</v-toolbar-title>
        </v-app-bar>
        <v-card-text class="pa-0">
          <!-- <div v-for="(entry, index) in tableMatiere" :key="index">
            <NuxtLink v-if="entry.level == 5" :to="entry.id">
              {{ entry.level }} {{ entry.title }} {{ entry.id }}
            </NuxtLink>
          </div> -->
          <v-treeview :items="tableMatiere" open-on-click :open="openArray">
            <template v-slot:label="{ item }">
              <NuxtLink v-if="item.children.length == 0" :to="item.id">
                {{ item.name }}
              </NuxtLink>
              <div v-else>
                {{ item.name }}
              </div>
            </template>
          </v-treeview>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  props: ["content"],
  data() {
    return {
      dialog: false,
      test: ["0", "0-2"]
    };
  },
  computed: {
    cec() {
      return this.$store.state.cec;
    },
    openArray() {
      let content = this.content.split("-");
      let openArray = [];
      let acc = "";
      for (const letter of content) {
        acc += "-" + letter;
        openArray.push(acc);
      }
      let final = [];
      for (const elem of openArray) {
        final.push(elem.substring(1));
      }
      console.log(final);
      return final;
    },
    tableMatiere() {
      let cec = this.cec;
      let tree = [];
      for (let p = 0; p < cec.length; p++) {
        tree.push({
          id: p,
          name: cec[p][0],
          children: []
        });
        for (let s = 2; s < cec[p].length; s++) {
          tree[p].children.push({
            id: String(p) + "-" + String(s),
            name: cec[p][s][0],
            children: []
          });
          for (let c = 2; c < cec[p][s].length; c++) {
            tree[p].children[s - 2].children.push({
              id: String(p) + "-" + String(s) + "-" + String(c),
              name: cec[p][s][c][0],
              children: []
            });
            for (let a = 2; a < cec[p][s][c].length; a++) {
              tree[p].children[s - 2].children[c - 2].children.push({
                id:
                  String(p) +
                  "-" +
                  String(s) +
                  "-" +
                  String(c) +
                  "-" +
                  String(a),
                name: cec[p][s][c][a][0],
                children: []
              });
              for (let para = 2; para < cec[p][s][c][a].length; para++) {
                tree[p].children[s - 2].children[c - 2].children[
                  a - 2
                ].children.push({
                  id:
                    String(p) +
                    "-" +
                    String(s) +
                    "-" +
                    String(c) +
                    "-" +
                    String(a) +
                    "-" +
                    String(para),
                  name: cec[p][s][c][a][para][0],
                  children: []
                });
                for (let r = 2; r < cec[p][s][c][a][para].length; r++) {
                  tree[p].children[s - 2].children[c - 2].children[
                    a - 2
                  ].children[para - 2].children.push({
                    id:
                      String(p) +
                      "-" +
                      String(s) +
                      "-" +
                      String(c) +
                      "-" +
                      String(a) +
                      "-" +
                      String(para) +
                      "-" +
                      String(r) +
                      "-12",
                    name: cec[p][s][c][a][para][r][0],
                    children: []
                  });
                }
              }
            }
          }
        }
      }
      return tree;
    }
  }
};
</script>
