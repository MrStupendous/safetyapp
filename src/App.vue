<template>
 <v-app>

    <v-main>
      <FormBuilder/>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import FormBuilder from './components/FormBuilder.vue';

export default {
  name: 'App',

  components: {
    FormBuilder,
  },

  data: () => ({
    //
  }),
};
</script>
