<template>
  <div class="card bg-dark text-white h-100 border-0">
    <!-- cover image -->
    <img :src="imgBaseURL + imgBaseDimension + img" :alt="title">

    <!-- film infos -->
    <div class="info d-none">

      <!-- layover color -->
      <div class="layover">
        <div class="info-text">
          <!-- TITOLO -->
          <h4>{{title}}</h4>

          <!-- TITOLO ORIGINALE -->
          <div class="original-title">
            Titolo originale = {{originalTitle}}
          </div>

          <!-- LINGUA -->
          <div class="language">
            Lingua = <img :src="getImgUrl(language)" v-bind:alt="language">
          </div>

          <!-- VOTO -->
          <div class="vote">
            Voto = 

            <!-- STELLE PIENE -->
            <i class="fas fa-star"
            v-for="(star,i) in loadStars(vote)" :key="'A' + i"
            ></i>

            <!-- STELLE VUOTE -->
            <i class="far fa-star"
            v-for="(star,i) in loadEmptyStars(vote)" :key="'B' + i"
            ></i>
          </div>
        </div>
      </div>

    </div>
  </div>

</template>

<script>
export default {
  name : 'CardGen',
  props : ['title','originalTitle','vote','language','imgBaseURL','imgBaseDimension','img'],
  methods : {
    getImgUrl(pic) {
      console.log(this.imgBaseURL + this.imgBaseDimension + this.img);
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
  .card{
    min-height: 150px;
  }
</style>