<template>
  <div id="app">
    <!-- inizio header -->
    <Header 
    :genreList="generalGenre"
    @startTest="srcInAPI"
    @setGenre="setGenre"/>
    <!-- fine header -->

    <!-- inizio main -->
    <Main
    :filmArray="filteredFilm" 
    :tvArray="filteredTv" 
    :imgBaseURL="imgBaseURL" 
    :imgBaseDimension="imgBaseDimension"
    :srcText="srcText"
    :filmCast="filteredFilmCast"
    :filmGenre="filteredFilmGenres"
    :tvCast="filteredTvCast"
    :tvGenre="filteredTvGenres"
    :genreFilter="setGenre"/>
    <!-- fine main -->
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
      genreFilter : '',
      filteredFilm : [],
      filteredFilmCast : [],
      filteredFilmGenres : [],
      filteredTv : [],
      filteredTvCast : [],
      filteredTvGenres : []
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
        // prepare film and tv series main api
        .all([axios.get(this.filmApiURL,params),axios.get(this.tvApiURL,params)])
        // save the answer from apis to main array and to filtered array to show them on page
        .then(axios.spread((responseFilm,responseTv) => {
          this.filmArray = responseFilm.data.results;
          this.filteredFilm = this.filmArray;
          this.tvArray = responseTv.data.results;
          this.filteredTv = this.tvArray;
        }))
        // catch the error from api if there are error and show them in the log
        .catch(axios.spread((errorFilm,errorTv) => {
          console.log('Errore API film : ' + errorFilm);
          console.log('Errore API serie TV : ' + errorTv);
        }))
        // when the main apis are fully loaded, do next cycle
        .finally(() => {
          // reset array for general genres list
          this.generalGenre = [];


          // reset the arrays of the films
          this.filmCast = [];
          let tempCast = [];
          this.filmGenre = [];
          let tempGenre = [];

          // call an api to load the cast and another one to load the genres of each film in the main array
          this.filmArray.forEach(item => {
            axios.all([
              axios.get(`https://api.themoviedb.org/3/movie/${item.id}/credits?api_key=eaf4c2856a7f976135b9da0ff4eb870a`),
              axios.get(`https://api.themoviedb.org/3/movie/${item.id}?api_key=eaf4c2856a7f976135b9da0ff4eb870a`)])
              .then(axios.spread((answerCast, answerGenre) => {
                // save the single film's cast in a temporary array and, when it is completely filled, 
                // save it into the main film cast array and in the filtered one to show them in the cards
                tempCast.push(answerCast.data.cast);
                if(tempCast.length == this.filmArray.length){
                  this.filmCast = tempCast;
                  this.filteredFilmCast = this.filmCast;
                }

                // save the single film's genres in a temporary array and, when it is completely filled, 
                // save it into the main film genres array and in the filtered one to show them in the cards
                tempGenre.push(answerGenre.data.genres);
                if(tempGenre.length == this.filmArray.length){
                  this.filmGenre = tempGenre;
                  this.filteredFilmGenres = this.filmGenre;
                }
              }))
              .catch((eC,eG) => {
                // catch eventually errors
                console.log('Film cast error : ' + eC);
                console.log('Film genres error : ' + eG);
              })
              // when the casts and genres apis are fully loaded, do next cycle
              .finally(() => {
                // create an array which contain all the genres of the films and tv series once per type
                this.filmGenre.forEach(element => {
                  if(element.length > 0){
                    element.forEach(genre => {
                      if(!this.generalGenre.includes(genre.name)){
                        this.generalGenre.push(genre.name);
                        this.generalGenre.sort();
                      }
                    });
                  }
                });
              });
          });


          // reset the arrays of the films
          this.tvCast = []
          let tempCast2 = []
          this.tvGenre = [];
          let tempGenre2 = [];

          // call an api to load the cast and another one to load the genres of each tv series in the main array
          this.tvArray.forEach(item => {
            axios.all([
              axios.get(`https://api.themoviedb.org/3/tv/${item.id}/credits?api_key=eaf4c2856a7f976135b9da0ff4eb870a`),
              axios.get(`https://api.themoviedb.org/3/tv/${item.id}?api_key=eaf4c2856a7f976135b9da0ff4eb870a`)])
              .then(axios.spread((answerCast, answerGenre) => {
                // save the single tv series' cast in a temporary array and, when it is completely filled, 
                // save it into the main tv series cast array and in the filtered one to show them in the cards
                tempCast2.push(answerCast.data.cast);
                if(tempCast2.length == this.tvArray.length){
                  this.tvCast = tempCast2;
                  this.filteredTvCast = this.tvCast;
                }

                // save the single tv series' genres in a temporary array and, when it is completely filled, 
                // save it into the main tv series genres array and in the filtered one to show them in the cards
                tempGenre2.push(answerGenre.data.genres);
                if(tempGenre2.length == this.tvArray.length){
                  this.tvGenre = tempGenre2;
                  this.filteredTvGenres = this.tvGenre;
                }
              }))
              .catch((eC,eG) => {
                // catch eventually errors
                console.log('Tv series cast error : ' + eC);
                console.log('Tv series genres error : ' + eG);
              })
              .finally(() => {
                // create an array which contain all the genres of the films and tv series once per type
                this.tvGenre.forEach(element => {
                  if(element.length > 0){
                    element.forEach(genre => {
                      if(!this.generalGenre.includes(genre.name)){
                        this.generalGenre.push(genre.name);
                        this.generalGenre.sort();
                      }
                    });
                  }
                });
              });
          })
        });
    },
    setGenre(genre){

      // Reset all filtered memory before changing them
      this.filteredFilm = [];
      this.filteredFilmCast = [];
      this.filteredFilmGenres = [];
      this.filteredTv = [];
      this.filteredTvCast = [];
      this.filteredTvGenres = [];

      setTimeout(() => {
        // check if the filter is not present and, in case, set it to ''
        typeof(genre) != "undefined" ? this.activeGenre = genre : '';

        // if there is no filter applied, use the main array to show films and series
        if(this.activeGenre == ''){
          this.filteredFilm = this.filmArray;
          this.filteredFilmCast = this.filmCast;
          this.filteredFilmGenres = this.filmGenre;
          this.filteredTv = this.tvArray;
          this.filteredTvCast = this.tvCast;
          this.filteredTvGenres = this.tvGenre;
        }
        else{
          // filter the films array using selected genre filter
          let temp = []
          this.filteredFilm = this.filmArray.filter((element,i) => {
            temp = [];
            this.filmGenre[i].forEach(element => {
              temp.push(element.name);
              // console.log(element);
            });

            if(temp.includes(this.activeGenre)){
              this.filteredFilmCast.push(this.filmCast[i]);
              this.filteredFilmGenres.push(this.filmGenre[i]);
            }
            return temp.includes(this.activeGenre);

          });

          // filter the tv series array using selected genre filter
          let temp2 = []
          this.filteredTv = this.tvArray.filter((element,i) => {
            temp2 = [];
            this.tvGenre[i].forEach(element => {
              temp2.push(element.name);
            });

            if(temp2.includes(this.activeGenre)){
              this.filteredTvCast.push(this.tvCast[i]);
              this.filteredTvGenres.push(this.tvGenre[i]);
            }

            return temp2.includes(this.activeGenre);
          });
        }
      }, 500);
    }
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
