<template>
  <main>
    <div class="container">
      <h2 v-if="srcText != ''">Risultati per la ricerca = "{{srcText}}"</h2>

      <!-- Cards per films -->
      <h3 v-if="filmArray.length != 0">Films</h3>
      <div class="filmCards">
        <CardFilm 
        v-for="(film,i) in filmArray"
        :key="i"
        :film="film"
        :imgBaseURL="imgBaseURL"
        :imgBaseDimension="imgBaseDimension"/>

      </div>

      <!-- Cards per serie TV -->
      <h3 v-if="tvArray.length != 0">Serie TV</h3>
      <div class="tvCards">
        <CardTv 
        v-for="(tv,i) in tvArray"
        :key="i"
        :tv="tv"
        :imgBaseURL="imgBaseURL"
        :imgBaseDimension="imgBaseDimension"/>

      </div>

    </div>
  </main>
</template>

<script>
import CardFilm from '@/components/CardFilm.vue';
import CardTv from '@/components/CardTv.vue';

export default {
  name : 'Main',
  components : {
    CardFilm,
    CardTv
  },
  props : ['filmArray','tvArray','imgBaseURL','imgBaseDimension','srcText']
}
</script>

<style lang="scss">
  main{
    padding: 30px 0;
    min-height: calc(100vh - 100px);
    background-color: #141414;
    color: white;

    h3{
      margin-top: 40px;
    }

    .filmCards,
    .tvCards{
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      flex-wrap: wrap;
      margin: 20px 0;
    }
  }
</style>