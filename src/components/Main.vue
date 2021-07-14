<template>
  <main>
    <div class="container">
      <h2 v-if="srcText != ''">Risultati per la ricerca = "{{srcText}}"</h2>

      <!-- Cards per films -->
      <h3 v-if="filmArray.length != 0">Films</h3>
      <div class="filmCards row row-cols-4 mb-5">
        <div v-for="(film,i) in filmArray" :key="i" class="g-5">
          <CardGen
          :title="film.title"
          :originalTitle="film.original_title"
          :vote="film.vote_average"
          :language="film.original_language"
          :overview="film.overview"
          :imgBaseURL="imgBaseURL"
          :imgBaseDimension="imgBaseDimension"
          :img="film.poster_path"
          :cast="filmCast[i]"
          :genres="filmGenre[i]"
          :genreFilter="genreFilter"
          />
        </div>

      </div>

      <!-- Cards per serie TV -->
      <h3 v-if="tvArray.length != 0">Serie TV</h3>
      <div class="tvCards row row-cols-4 mb-5">
        <div v-for="(tv,i) in tvArray" :key="i"  class="g-5">
          <CardGen
          :title="tv.name"
          :original-title="tv.original_name"
          :vote="tv.vote_average"
          :language="tv.original_language"
          :overview="tv.overview"
          :imgBaseURL="imgBaseURL"
          :imgBaseDimension="imgBaseDimension"
          :img="tv.poster_path"
          :cast="tvCast[i]"
          :genres="tvGenre[i]"
          />
        </div>
      </div>

    </div>
  </main>
</template>

<script>
import CardGen from '@/components/CardGen.vue';

export default {
  name : 'Main',
  components : {
    CardGen
  },
  props : ['filmArray','tvArray','imgBaseURL','imgBaseDimension','srcText','filmCast','filmGenre','tvCast','tvGenre','genreFilter']
}
</script>

<style lang="scss">
  main{
    // padding: 30px 0;
    // min-height: calc(100vh - 100px);
    background-color: #141414;
    color: white;

    .container > h3{
      margin-top: 40px;
      font-size: 2rem;
      text-transform: uppercase;
    }
  }
</style>