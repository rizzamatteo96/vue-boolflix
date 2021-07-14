<template>
  <div id="app">
    <Header @startTest="srcInAPI"/>
    <Main
    :filmArray="filmArray" 
    :tvArray="tvArray" 
    :imgBaseURL="imgBaseURL" 
    :imgBaseDimension="imgBaseDimension"
    :srcText="srcText"
    :filmCast="filmCast"
    :filmGenre="filmGenre"
    :tvCast="tvCast"
    :tvGenre="tvGenre"/>
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
      srcText : '',
      generalGenre : [],
      filmCast : [],
      filmGenre : [],
      tvCast : [],
      tvGenre : [],
    }
  },
  methods : {
    srcInAPI(inputText){
      // save user search text
      this.srcText = inputText;
      
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
        }))
        .finally(() => {
          generalGenre = [];

          this.filmCast = [];
          let tempCast = [];
          this.filmGenre = [];
          let tempGenre = [];
          this.filmArray.forEach(item => {
            axios.all([
              axios.get(`https://api.themoviedb.org/3/movie/${item.id}/credits?api_key=eaf4c2856a7f976135b9da0ff4eb870a`),
              axios.get(`https://api.themoviedb.org/3/movie/${item.id}?api_key=eaf4c2856a7f976135b9da0ff4eb870a`)])
              .then(axios.spread((answerCast, answerGenre) => {
                tempCast.push(answerCast.data.cast);
                if(tempCast.length == this.filmArray.length){
                  this.filmCast = tempCast;
                  console.log(this.filmCast);
                }

                tempGenre.push(answerGenre.data.genres);
                if(tempGenre.length == this.filmArray.length){
                  this.filmGenre = tempGenre;
                  console.log(this.filmGenre);
                }
              }))
              .catch((e) => {
                console.log('Film cast error : ' + e);
              })
              .finally(() => {

              });
          });

          this.tvCast = []
          let tempCast2 = []
          this.tvGenre = [];
          let tempGenre2 = [];
          this.tvArray.forEach(item => {
            axios.all([
              axios.get(`https://api.themoviedb.org/3/tv/${item.id}/credits?api_key=eaf4c2856a7f976135b9da0ff4eb870a`),
              axios.get(`https://api.themoviedb.org/3/tv/${item.id}?api_key=eaf4c2856a7f976135b9da0ff4eb870a`)])
              .then(axios.spread((answerCast, answerGenre) => {
                tempCast2.push(answerCast.data.cast);
                if(tempCast2.length == this.tvArray.length){
                  this.tvCast = tempCast2;
                  console.log(this.tvCast);
                }

                tempGenre2.push(answerGenre.data.genres);
                if(tempGenre2.length == this.tvArray.length){
                  this.tvGenre = tempGenre2;
                  console.log(this.tvGenre);
                }
              }))
              .catch((e) => {
                console.log('Film cast error : ' + e);
              })
              .finally(() => {});
          })
        });
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
