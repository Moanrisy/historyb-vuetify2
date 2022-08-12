<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/logo_size.jpg')"
          class="my-3"
          contain
          height="400"
        />
      </v-col>
    </v-row>
    <template>
      <v-card color="blue lighten-2" dark>
        <v-card-title class="text-h5 blue lighten-3">
          Search for Historical Building
        </v-card-title>
        <!-- <v-card-text>
          Explore hundreds of free API's ready for consumption! For more
          information visit
          <a
            class="grey--text text--lighten-3"
            href="https://github.com/toddmotto/public-apis"
            target="_blank"
            >the GitHub repository</a
          >.
        </v-card-text> -->
        <v-card-text>
          <v-autocomplete
            v-model="model"
            :items="items"
            :loading="isLoading"
            :search-input.sync="search"
            color="white"
            hide-no-data
            hide-selected
            item-text="Description"
            item-value="API"
            label="Building name"
            placeholder="Start typing to Search"
            prepend-icon="mdi-home-modern"
            return-object
          ></v-autocomplete>
        </v-card-text>
        <v-divider></v-divider>
        <v-expand-transition>
          <v-list v-if="model" class="red lighten-3">
            <v-row>
              <v-col cols="2">
                <v-img :src="model.image"> </v-img>
                <View360Dialog />
                <View3DDialog />
              </v-col>
              <v-col>
                <v-list-item v-for="(field, i) in fields" :key="i">
                  <v-list-item-content>
                    <v-list-item-title v-text="field.value"></v-list-item-title>
                    <v-list-item-subtitle
                      v-text="field.key"
                    ></v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-col>
            </v-row>
          </v-list>
        </v-expand-transition>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn :disabled="!model" color="grey darken-3" @click="model = null">
            Clear
            <v-icon right> mdi-close-circle </v-icon>
          </v-btn>
        </v-card-actions>
      </v-card>
    </template>
  </v-container>
</template>

<script lang="ts">
import Vue from "vue";
// import View360Mode from "./View360Mode.vue";
import View360Dialog from "./View360Dialog.vue";
import View3DDialog from "./View3DDialog.vue";

export default Vue.extend({
  name: "HelloWorld",

  data: () => ({
    descriptionLimit: 60,
    entries: [],
    isLoading: false,
    model: null,
    search: null,
    dialog: false,
  }),
  components: {
    View360Dialog,
    View3DDialog,
  },
  methods: {
    view360() {
      alert("view in 360");
    },
  },
  computed: {
    fields() {
      if (!this.model) return [];

      return Object.keys(this.model).map((key) => {
        return {
          key,
          value: this.model[key] || "n/a",
        };
      });
    },
    items() {
      return this.entries.map((entry) => {
        // const Description =
        //   entry.Description.length > this.descriptionLimit
        //     ? entry.Description.slice(0, this.descriptionLimit) + "..."
        //     : entry.Description;
        const Description = entry.title;

        return Object.assign({}, entry, { Description });
      });
    },
  },
  watch: {
    model() {
      console.log("model watched");
    },
    search() {
      // Items have already been loaded
      if (this.items.length > 0) return;

      // Items have already been requested
      if (this.isLoading) return;

      this.isLoading = true;

      // Lazily load input items
      // fetch("https://api.publicapis.org/entries")
      // fetch("https://my-json-server.typicode.com/typicode/demo/posts")
      fetch("http://localhost:8010/proxy/buildings/")
        .then((res) => res.json())
        .then((res) => {
          this.entries = res;
          // const { count, entries } = res;
          // this.count = count;
          // this.entries = entries;
          // console.log(count);
        })
        .catch((err) => {
          console.log(err);
        })
        .finally(() => (this.isLoading = false));
    },
  },
});
</script>
