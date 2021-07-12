<template>
  <div>
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

</style>