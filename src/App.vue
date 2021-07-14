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
    :tvCast="tvCast"/>
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
      filmCast : [],
      tvCast : []
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
          this.filmCast = []
          let temp = []
          this.filmArray.forEach(item => {
            axios
              .get(`https://api.themoviedb.org/3/movie/${item.id}/credits?api_key=eaf4c2856a7f976135b9da0ff4eb870a`)
              .then((answer) => {
                temp.push(answer.data.cast);
                if(temp.length == this.filmArray.length){
                  this.filmCast = temp;
                  console.log(this.filmCast);
                }
              })
              .catch((e) => {
                console.log('Film cast error : ' + e);
              })
              .finally(() => {});
          });

          this.tvCast = []
          let temp2 = []
          this.tvArray.forEach(item => {
            axios
              .get(`https://api.themoviedb.org/3/tv/${item.id}/credits?api_key=eaf4c2856a7f976135b9da0ff4eb870a`)
              .then((answer) => {
                temp2.push(answer.data.cast);
                if(temp2.length == this.tvArray.length){
                  this.tvCast = temp2;
                  console.log(this.tvCast);
                }
              })
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
