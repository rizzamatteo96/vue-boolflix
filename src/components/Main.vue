<template>
  <main>

    <h2 v-if="srcText != ''">Risultati per la ricerca = {{srcText}}</h2>

    <div class="filmCard">

      <Card 
      v-for="(film,i) in filmArray"
      :key="i"
      :film="film"/>

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
    min-height: calc(100vh - 100px);
    background-color: #141414;
    color: white;
  }
</style>