<template>
  <div id="app">
    <Header @startTest="srcInAPI"/>
    <Main 
    :filmArray="filmArray" 
    :tvArray="tvArray" 
    :imgBaseURL="imgBaseURL" 
    :imgBaseDimension="imgBaseDimension"
    :srcText="srcText"/>
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
      filmApiURL : 'https://api.themoviedb.org/3/search/movie?',
      tvApiURL : 'https://api.themoviedb.org/3/search/tv?',
      filmArray : [],
      tvArray : [],
      imgBaseURL : 'https://image.tmdb.org/t/p/',
      imgBaseDimension : 'original',
      srcText : ''
    }
  },
  methods : {
    srcInAPI(inputText){
      this.srcFilm(inputText);
      this.srcTv(inputText);
    },
    srcFilm(srcText){
      // save user search text
      this.srcText = srcText;

      // call films API
      axios
        .get(this.filmApiURL, {
          params: {
            api_key : 'eaf4c2856a7f976135b9da0ff4eb870a',
            query : srcText,
            language : 'it-IT'
          }
        })
        .then(response => {
          // console.log(response.data.results);
          this.filmArray = response.data.results;
          // console.log(this.filmArray);
        })
        .catch((error) => {
          console.log('Errore : ' + error);
        });
    },
    srcTv(srcText){
      // save user search text
      this.srcText = srcText;

      // call films API
      axios
        .get(this.tvApiURL, {
          params: {
            api_key : 'eaf4c2856a7f976135b9da0ff4eb870a',
            query : srcText,
            language : 'it-IT'
          }
        })
        .then(response => {
          // console.log(response.data.results);
          this.tvArray = response.data.results;
          // console.log(this.filmArray);
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
