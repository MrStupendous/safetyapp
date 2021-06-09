<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <FormBuilder msg="Welcome to Your Vue.js + TypeScript App"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import FormBuilder from '@/components/FormBuilder.vue'; // @ is an alias to /src

@Component({
  components: {
    FormBuilder,
  },
})
export default class Home extends Vue {}
</script>
