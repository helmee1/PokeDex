<template>
  <div class="container">
    <h1>Pokemon's</h1>
    <PokemonList 
      :imageUrl="imageUrl" 
      :apiUrl="apiUrl"
      @setPokemonUrl="setPokemonUrl" /> 
    <PokemonDetail
    v-if="showDetail"  
    :pokemonUrl="pokemonUrl"
    :imageUrl="imageUrl"
    @closeDetail="closeDetail"
    /> 
      <!--pokelist terus project image dgn api,and set pokeurl tu if tekan --> 
      <!--if showdetail true, kluarkan url poke dgn image,then if @click close detail,then close detail -->

  </div>
</template>

<script>
  import PokemonList from './PokemonList.vue'; //kita import component list
  import PokemonDetail from './PokemonDetail.vue'; //kita import component detail

  export default {
    data: () => {
      return {
        imageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
        apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
        showDetail: false,
        pokemonUrl : ''
      }
    },
    components: {
      PokemonList,
      PokemonDetail
    },
    methods: {
      setPokemonUrl(url) {
        this.pokemonUrl = url;
        this.showDetail = true;
      },
      closeDetail() {
        this.pokemonUrl = '';
        this.showDetail = false;
      },//function untuk close detail

    }
  } //kita boleh export data2 dalam ni untuk digunakan dalam component lain
</script>

<style lang="scss" scoped> //sassy css


  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    padding: 10px;
    width: calc(100% - 20px);
    min-height: calc(100vh - 20px);
    background: blue;

    font-family:arial;
    font-size: 1rem;
    font-weight: normal;
  }

  h1 { color: #ff0000; }
</style>
