<template>
  <v-app>
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
