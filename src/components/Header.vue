<template>
  <header>
    <div class="left">
      <h1>
        Boolflix
      </h1>
    </div>

    <div class="right">
      <!-- inizio selezione generi -->
      <select @change="setGenreSrc($event)" v-if="genreList.length > 0" name="genres" id="genres">
        <option value="">Seleziona un genere dalla lista</option>
        <option 
        v-for="(genre,i) in genreList" 
        :key="i" 
        :value="genre"> {{genre}} </option>
      </select>
      <!-- fine selezione generi -->

      <!-- inizio sezione di ricerca -->
      <form action="">
        <input 
        type="text" 
        placeholder="Inserisci la tua ricerca..."
        v-model.trim="srcText"
        >
        <button @click.prevent="$emit('startTest',srcText)">Cerca</button>
      </form>
      <!-- fine sezione di ricerca -->
    </div>
  </header>
</template>

<script>
export default {
  name : 'Header',
  props : ['genreList'],
  data(){
    return{
      srcText : ''
    }
  },
  methods : {
    setGenreSrc(event){
      // console.log(event.target.value);
      this.$emit('setGenre', event.target.value);
    }
  }
}
</script>

<style lang="scss">
  header{
    display: flex;
    justify-content: space-between;
    align-items: center;

    background-color: #0d0d0d;
    height: 100px;
    padding: 0 20px;

    .left{
      text-transform: uppercase;
      color: red;
      font-size: 1.5rem;
      font-weight: 700;
    }

    .right{
      form{
        *{
          padding: 10px 15px;
          margin: 0 5px;
        }
      }
    }
  }
</style>