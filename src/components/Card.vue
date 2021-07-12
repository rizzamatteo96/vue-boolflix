<template>
  <div class="card">
    Titolo = {{film.title}} <br>
    Titolo originale = {{film.original_title}} <br>
    Lingua = {{film.original_language}} <br>
    Voto = {{film.vote_average}} <br>
  </div>
</template>

<script>
export default {
  name : 'Card',
  props : ['film']
}
</script>

<style lang="scss">
  .card{
    width: calc(100% / 5);
    background-color: gray;
    border-radius: 20px;
    border: 1px solid red;
    padding: 20px;
  }
</style>