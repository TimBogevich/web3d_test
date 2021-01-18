<template>
  <v-app>
    <v-main>
      <test/>
    </v-main>
  </v-app>
</template>

<script>
import test from './components/test';

export default {
  name: 'App',

  components: {
    test,
  },

  data: () => ({
    //
  }),
};
</script>
