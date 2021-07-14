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
      imgBaseDimension : 'w342',
      srcText : ''
    }
  },
  methods : {
    srcInAPI(inputText){
      // save user search text
      this.srcText = srcText;
      
      // prepare query parameters
      const srcParams = {
          params: {
            api_key : 'eaf4c2856a7f976135b9da0ff4eb870a',
            query : inputText,
            language : 'it-IT'
          }
        };

      this.callAxios(srcParams);
    },
    callAxios(params){
      // call APIs
      axios
        .all([axios.get(this.filmApiURL,params),axios.get(this.tvApiURL,params)])
        .then(axios.spread((responseFilm,responseTv) => {
          this.filmArray = responseFilm.data.results;
          this.tvArray = responseTv.data.results;
        }))
        .catch(axios.spread((errorFilm,errorTv) => {
          console.log('Errore API film : ' + errorFilm);
          console.log('Errore API serie TV : ' + errorTv);
        }));
    },
    
  }
}
</script>

<style lang="scss">
  @import '@/style/commons.scss';
  @import '@/style/cards.scss';
  
  body{
    background-color: #141414;
  }

</style>
