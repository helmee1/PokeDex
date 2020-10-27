<template>
  <div class="list">
    <article v-for="(pokemon, index) in pokemons"
    :key="'poke'+index"
    @click="setPokemonUrl(pokemon.url)"> 
      <img :src="imageUrl + pokemon.id + '.png'" width="96" height="96" alt=""> <!--gmbr poke n nama-->
      <h3>{{ pokemon.name }} {{pokemon.height}}</h3> <!--extract nama pokemon dari url -->
    </article>


  </div>
</template>

<script>
  export default {
    props: [
      'imageUrl',
      'apiUrl'
    ],
    data: () => {
      return {
        pokemons: [],
      }
    },
    methods: {
      fetchData() {
        let req = new Request(this.currentUrl);
        fetch(req)
          .then((resp) => {
            if(resp.status === 200)//200 meaning process tu understood and received -- most common response in html
              return resp.json();//meaning dia kluarkan json response
              
          })
          .then((data) => {
            console.log(data.results)
            data.results.forEach(pokemon => {
              pokemon.id = pokemon.url.split('/')
                .filter(function(part) { return !!part }).pop(); //ni dia just nk amik n asingkan pokemon id n push id tu jadi object sama as nama n url
              this.pokemons.push(pokemon);//dia push semua pokemon punya id n data results masuk dalam array kt props
            });
            console.log(this.pokemons)//meaning value pokemons array ni dia bawa id,nama,url untuk setiap pokmeon
          })
          .catch((error) => {
            console.log(error); //nak catch any error--then dia console
          })
      },
      setPokemonUrl(url) {
        this.$emit('setPokemonUrl', url);
      }, //method ni amik dari pokemon vue,so dia akan show jadi poke.url = url and showdetail = true
    },

    created() {
      this.currentUrl = this.apiUrl;
      this.fetchData();
      console.log(this.apiUrl) //apiUrl ni bawa api,semua value pokemon tu..
    },
    // mounted() { //dia akan trigger whenever ada changes in data -- data change -> rerender n repatch -> update UI
    //   this.scrollTrigger();
    // }
  }
</script>

<style lang="scss" scoped>
  .list {
    display: grid; 
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    grid-gap: 10px;
    width: 100%;
    max-width: 510px;//buat dia jadi bentuk kotak2

    article {
      height: 150px;
      background-color: #f99696; //colour belakang each kotak
      text-align: center;
      text-transform: capitalize;
      border-radius: 5px;
      cursor: pointer;
      h3 {
        margin: 0;
      }
    }
  }

</style>

