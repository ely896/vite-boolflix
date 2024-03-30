<script>
import axios from 'axios';
import SearchMovie from './SearchMovie.vue';

export default {
  name: 'AppMain',
  components: {
    SearchMovie
  }

};
</script>

<template>
  <main>
    
    <SearchMovie></SearchMovie>
   
  </main>



</template>

<style scoped>
main {
  min-height: 500px;
}

</style>
