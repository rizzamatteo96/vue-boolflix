<template>
  <main>
    <div class="container">
      <h2 v-if="srcText != ''">Risultati per la ricerca = {{srcText}}</h2>

      <div class="filmCards">

        <Card 
        v-for="(film,i) in filmArray"
        :key="i"
        :film="film"/>

      </div>
    </div>
  </main>
</template>

<script>
import Card from '@/components/Card.vue';

export default {
  name : 'Main',
  components : {
    Card
  },
  props : ['filmArray', 'srcText']
}
</script>

<style lang="scss">
  main{
    padding: 30px 0;
    min-height: calc(100vh - 100px);
    background-color: #141414;
    color: white;

    .filmCards{
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      flex-wrap: wrap;
    }
  }
</style>