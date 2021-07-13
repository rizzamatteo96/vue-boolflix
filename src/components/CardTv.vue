<template>
  <div class="card pos-rel">
    <!-- cover image -->
    <img :src="imgBaseURL + imgBaseDimension + tv.poster_path" :alt="tv.name">

    <!-- Tv series info -->
    <div class="info d-none">

      <!-- layover color -->
      <div class="layover">
        <div class="info-text">
          <!-- TITOLO -->
          <h3>{{tv.name}}</h3>

          <!-- TITOLO ORIGINALE -->
          <div class="original-title">
            Titolo originale = {{tv.original_name}}
          </div>

          <!-- LINGUA -->
          <div class="language">
            Lingua = <img :src="getImgUrl(language)" v-bind:alt="language"> <br>
          </div>

          <!-- VOTO -->
          <div class="vote">
            Voto = 

            <!-- STELLE PIENE -->
            <i class="fas fa-star"
            v-for="(star,i) in loadStars(tv.vote_average)" :key="'A' + i"
            ></i>

            <!-- STELLE VUOTE -->
            <i class="far fa-star"
            v-for="(star,i) in loadEmptyStars(tv.vote_average)" :key="'B' + i"
            ></i>
          </div>
        </div>
      </div>

    </div>
  </div>

</template>

<script>
export default {
  name : 'CardTv',
  props : ['tv','imgBaseURL','imgBaseDimension'],
  data(){
    return{
      language : this.tv.original_language
    }
  },
  methods : {
    getImgUrl(pic) {
      // Funzione che verifica se l'immagine che si desidera caricare è presente
      try {
        let fileName = require('../assets/img/' + pic + '.png');
        // console.log("file found");
        return fileName
      } 
      catch (e) {
        // console.log("sorry, file not found" + e);
        return 'nothing'
      }  
    },
    loadStars(vote){
      // funzione che restituisce la quantità di stelle "piene" da stampare
      return parseInt(Math.round(vote / 2))
    },
    loadEmptyStars(vote){
      // funzione che restituisce la quantità di stelle "vuote" da stampare
      return 5 - parseInt((Math.round(vote / 2)))
    }
  }
}
</script>

<style lang="scss">

</style>