<template>
  <v-app>
    <v-app-bar app color="#5cb071" dark>
      <div class="d-flex align-center"></div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <v-fence />
    </v-main>
  </v-app>
</template>

<script>
import vFence from "./components/v-fence";

export default {
  name: "App",

  components: {
    vFence
  },

  data: () => ({
    //
  })
};
</script>
