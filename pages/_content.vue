<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="px-4">
        <div class="text-body-1 font-weight-bold my-4 text-center">
          {{ paragrapheTitle }}
        </div>

        <v-row justify="space-around" align="center" class="mb-1">
          <v-col cols="4" class="text-center">
            <Navigation></Navigation>
          </v-col>
          <v-col cols="4" class="text-center" v-if="paragraphe[1].length > 0">
            <Introduction :intro="paragraphe[1]"></Introduction>
          </v-col>
          <v-col cols="4" class="text-center">
            <v-chip color="primary" :outlined="isEnbref" @click="gotoEnbref">
              En bref
            </v-chip>
          </v-col>
        </v-row>

        <div class="text-center">
          <v-pagination
            v-model="referenceNumber"
            :length="referenceLength"
          ></v-pagination>
        </div>

        <div
          v-for="(ligne, index) in lignes"
          :key="index"
          class="my-4 text-justify"
        >
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
export default {
  layout: "subpage",

  async asyncData({ params }) {
    const content = params.content; // When calling /abc the slug will be "abc"
    return { content };
  },

  data: () => ({
    showIntro: false
  }),

  methods: {
    gotoEnbref() {
      console.log("HEY");
      this.referenceNumber = this.referenceLength;
    }
  },

  computed: {
    cec() {
      return this.$store.state.cec;
    },
    paragrapheTitle() {
      let id = this.content.split("-");
      let partie = Number(id[0]);
      let section = Number(id[1]);
      let chapitre = Number(id[2]);
      let article = Number(id[3]);
      let paragraphe = Number(id[4]);
      return this.cec[partie][section][chapitre][article][paragraphe][0];
    },
    referenceNumber: {
      // getter
      get: function() {
        let id = this.content.split("-");
        let reference = Number(id[5]);
        return reference - 1;
      },
      set: function(newValue) {
        let id = this.content.split("-");
        id.pop();
        id.pop();
        id.push(String(newValue + 1));
        let newId = id.join("-") + "-12";
        console.log(newId);
        this.$router.push("/" + newId);
      }
    },
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
    },
    paragraphe() {
      let id = this.content.split("-");
      let partie = Number(id[0]);
      let section = Number(id[1]);
      let chapitre = Number(id[2]);
      let article = Number(id[3]);
      let paragraphe = Number(id[4]);

      //console.log(this.cec[partie][section][chapitre][article][paragraphe]);

      return this.cec[partie][section][chapitre][article][paragraphe];
    },
    referenceLength() {
      let id = this.content.split("-");
      let partie = Number(id[0]);
      let section = Number(id[1]);
      let chapitre = Number(id[2]);
      let article = Number(id[3]);
      let paragraphe = Number(id[4]);

      return (
        this.cec[partie][section][chapitre][article][paragraphe].length - 2
      );
    },
    isEnbref() {
      if (this.referenceNumber == this.referenceLength) {
        return false;
      } else {
        return true;
      }
    }
  }
};
</script>
