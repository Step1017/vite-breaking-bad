<script>
  import AppHeader from './components/AppHeader.vue';
  import AppMain from './components/AppMain.vue';
  import axios from 'axios';
  import {store} from '../src/store';
  export default {
    name: 'App',
    components: {
      AppHeader,
      AppMain
    },
    data() {
      return {
        store,
      }
    },
    created() {
      axios.get('https://db.ygoprodeck.com/api/v7/cardinfo.php').then((response) => {
        this.store.cards = response.data.data.slice(0,20);
      });
    }
  }
</script>

<template>
  <AppHeader/>
  <AppMain/>
</template>

<style lang="scss">
  @import "./styles/main.scss";
</style>
