<template>
  <div id="app">
    <Header @startTest="srcFilm"/>
    <Main :filmArray="filmArray" :srcText="srcText"/>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Main from './components/Main.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    Header,
    Main
  },
  data(){
    return{
      apiURL : 'https://api.themoviedb.org/3/search/movie?',
      filmArray : [],
      srcText : ''
    }
  },
  methods : {
    srcFilm(inputText){

      // save user search text
      this.srcText = inputText;

      // call films API
      axios
        .get(this.apiURL, {
          params: {
            api_key : 'eaf4c2856a7f976135b9da0ff4eb870a',
            query : inputText,
            language : 'it-IT'
          }
        })
        .then(response => {
          // console.log(response.data.results);
          this.filmArray = response.data.results;
          console.log(this.filmArray);
        })
        .catch((error) => {
          console.log('Errore : ' + error);
        });
    }
  }
}
</script>

<style lang="scss">
@import '@/style/commons.scss';


</style>
