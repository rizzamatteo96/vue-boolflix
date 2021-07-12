<template>
  <header>
    <div class="left">
      <h1>
        Boolflix
      </h1>
    </div>

    <div class="right">
      <form action="">
        <input type="text" placeholder="Inserisci la tua ricerca...">
        <button>Cerca</button>
      </form>
    </div>
  </header>
</template>

<script>
export default {
  name : 'Header'
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