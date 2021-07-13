<template>
  <div class="card pos-rel">
    <!-- cover image -->
    <img :src="imgBaseURL + imgBaseDimension + film.backdrop_path" :alt="film.title">

    <!-- film infos -->
    <div class="info d-none">

      <!-- layover color -->
      <div class="layover">
        <div class="info-text">
          Titolo = {{film.title}} <br>
          Titolo originale = {{film.original_title}} <br>
          Lingua = <img :src="getImgUrl(language)" v-bind:alt="language"> <br>
          Voto = 

          <!-- fill stars -->
          <i class="fas fa-star"
          v-for="(star,i) in loadStars(film.vote_average)" :key="'A' + i"
          ></i>

          <!-- empty stars -->
          <i class="far fa-star"
          v-for="(star,i) in loadEmptyStars(film.vote_average)" :key="'B' + i"
          ></i>
        </div>
      </div>

    </div>
  </div>

</template>

<script>
export default {
  name : 'CardFilm',
  props : ['film','imgBaseURL','imgBaseDimension'],
  data(){
    return{
      language : this.film.original_language
    }
  },
  methods : {
    getImgUrl(pic) {
      try {
        let fileName = require('../assets/img/' + pic + '.png');
        // console.log("file found");
        // console.log(fileName);
        return fileName
      } 
      catch (e) {
        // console.log("sorry, file not found" + e);
        return 'nothing'
      }  
    },
    loadStars(vote){
      return parseInt(Math.round(vote / 2))
    },
    loadEmptyStars(vote){
      return 5 - parseInt(Math.round(vote / 2))
    }
  }
}
</script>

<style lang="scss">
  
</style>