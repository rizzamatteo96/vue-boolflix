<template>
  <div class="card">
    <div class="cover">
      <img :src="imgBaseURL + imgBaseDimension + tv.poster_path" :alt="'Copertina ' + tv.name">
    </div>
    Percorso img = {{tv.poster_path}} <br>
    Titolo = {{tv.name}} <br>
    Titolo originale = {{tv.original_name}} <br>
    Lingua = <img :src="getImgUrl(language)" v-bind:alt="language"> <br>
    Voto = {{tv.vote_average}} <br>
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
    }
  }
}
</script>

<style lang="scss">
  .card{
    width: calc(100% / 5);
    background-color: gray;
    border-radius: 20px;
    border: 1px solid red;
    padding: 20px;
    
    .cover{
      img{
        height: 5rem;
      }
    }

    img{
      height: 1rem;
    }
  }
</style>