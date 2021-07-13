<template>
  <div class="card">
    Titolo = {{tv.name}} <br>
    Titolo originale = {{tv.original_name}} <br>
    Lingua = <img :src="getImgUrl(language)" v-bind:alt="language"> <br>
    Voto = {{tv.vote_average}} <br>
  </div>

</template>

<script>
export default {
  name : 'CardTv',
  props : ['tv'],
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

    img{
      height: 1rem;
    }
  }
</style>