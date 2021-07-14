<template>
  <div class="card bg-dark text-white h-100 border-0">
    <!-- cover image -->
    <img v-if="!imgError" 
    :src="imgBaseURL + imgBaseDimension + img" 
    :alt="title" 
    @error="imgError=true">
    <img v-else
    src="../assets/img/notFound.png" 
    alt="img not found">

    <!-- film infos -->
    <div class="info d-none">

      <!-- layover color -->
      <div class="layover">
        <div class="info-text">
          <!-- TITOLO -->
          <h4>{{title}}</h4>

          <!-- TITOLO ORIGINALE -->
          <div class="original-title">
            <span class="fw-bold">Titolo originale</span> = {{originalTitle}}
          </div>

          <!-- LINGUA -->
          <div class="language">
            <span class="fw-bold">Lingua </span> = <img :src="getImgUrl(language)" v-bind:alt="language">
          </div>

          <!-- VOTO -->
          <div class="vote">
            <span class="fw-bold">Voto</span> = 

            <!-- STELLE PIENE -->
            <i class="fas fa-star"
            v-for="(star,i) in loadStars(vote)" :key="'A' + i"
            ></i>

            <!-- STELLE VUOTE -->
            <i class="far fa-star"
            v-for="(star,i) in loadEmptyStars(vote)" :key="'B' + i"
            ></i>
          </div>

          <!-- OVERVIEW -->
          <div v-if="overview" class="overview">
            <span class="fw-bold">Overview</span> = {{overview}}
          </div>

          <!-- CAST -->
          <div v-if="cast && cast.length != 0" class="cast">
             <span class="fw-bold">Cast =</span>
             <ul>
               <li v-for="(item,i) in Math.min(5,cast.length)" :key="'C' + i"> {{cast[i].name}} </li>
             </ul>
          </div>

          <!-- GENRE -->
           <div v-if="genres && genres.length != 0" class="genres">
             <span class="fw-bold">Genres =</span>
             <ul>
               <li v-for="(item,i) in Math.min(5,genres.length)" :key="'C' + i"> {{genres[i].name}} </li>
             </ul>
          </div>
        </div>
      </div>

    </div>
  </div>

</template>

<script>
export default {
  name : 'CardGen',
  props : ['title','originalTitle','vote','language','overview','imgBaseURL','imgBaseDimension','img','idCard','cast','genres'],
  data(){
    return{
      imgError : false
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