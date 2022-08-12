<template>
  <v-dialog
    v-model="dialog"
    fullscreen
    hide-overlay
    transition="dialog-bottom-transition"
  >
    <template v-slot:activator="{ on, attrs }">
      <v-btn dark v-bind="attrs" v-on="on">View in 3D</v-btn>
    </template>
    <v-card>
      <v-toolbar dark color="primary">
        <v-btn icon dark @click="dialog = false">
          <v-icon>mdi-close</v-icon>
        </v-btn>
        <v-toolbar-title>3D Views</v-toolbar-title>
      </v-toolbar>

      <View3DMode />
    </v-card>
  </v-dialog>
</template>

<script>
import Vue from "vue";
import View3DMode from "./View3DMode.vue";

export default Vue.extend({
  name: "View3DDialog",

  data: () => ({
    descriptionLimit: 60,
    entries: [],
    isLoading: false,
    model: null,
    search: null,
    dialog: false,
  }),
  components: {
    View3DMode,
  },
});
</script>
