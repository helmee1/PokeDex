
<template>
  <div class="detail">
    <div class="detail-view" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="imageUrl + pokemon.id + '.png'" alt="">
      </div>
      <div v-if="pokemon" class="data">
        <h2>{{ pokemon.name }}</h2>
        <div class="property">
          <div class="left">Base Experience</div>
          <div class="right">{{ pokemon.base_experience }} XP</div>
        </div>
        <div class="property">
          <div class="left">Height</div>
          <div class="right">{{ pokemon.height }} m</div><!--project height dia then tambah ayat kg -->
        </div>
        <div class="property">
          <div class="left">Weight</div>
          <div class="right">{{ pokemon.weight }} kg</div> <!--project weight dia then tambah ayat kg -->
        </div>
        <h3>Pokemon Types</h3>
        <div class="types">
          <div class="type" 
            v-for="(value, index) in pokemon.types" 
            :key="'value'+index">
            {{ value.type.name }}
          </div><!--untuk project each value yg ada dalam name,sbb ada yg 2 name (poke type)-->
        </div>
        <h3>Abilities</h3>
        <div class="abilities">
          <div class="ability" 
            v-for="(value, index) in pokemon.abilities"
            :key="'value'+index">
            {{ value.ability.name }}
          </div><!--asalkan ada v-for,meaning kita nk loop n kluarkan each value untuk setiap index -->
        </div>
      </div>
      <h2 v-else>The pokemon was not found</h2>
      <button class="close" @click="closeDetail">close</button> <!--close button -->
    </div>
    <i v-else class="fas fa-spinner fa-spin"></i> <!--if semua kt atas tak jadi,then kita kluarkan spinner ni,nmpk loading -->
  </div>
</template>

<script>
  export default {
    props: [
      'pokemonUrl',
      'imageUrl'
    ],//custom props
    data: () => {
      return {
        show: false,//so bukak2 takde terus keluar detail
        pokemon: {} //letak as object 
      }      
    },
    methods: {
      fetchData() {
        let req = new Request(this.pokemonUrl); //jadikan req as variable baru yg contain api url tu
        fetch(req)
          .then((resp) => {
            if(resp.status === 200) //ok n received = then dpt json.
              return resp.json();
          })
          .then((data) => { //inject data yg dpt dari pokemonUrl,ke dalam this.pokemon
            this.pokemon = data;
            this.show = true; //then baru show data tu
            console.log(this.pokemon) //bila dpt pokemon url tu baru kluar data pokemon.
          })
          .catch((error) => {
            console.log(error);
          })//catch any error
      },
      closeDetail() {
        this.$emit('closeDetail');
      } //amik fx dari pokemon.vue
    },
    created() {
      this.fetchData();
    }//untuk jadikan dia reactive,sbb if takde,meaning takde response
  }
</script>

<style lang="scss" scoped>
  .detail { //ni the whole background lepas kita click kat any div
    display: flex; //kotak
    justify-content: center; //position
    align-items: flex-start;//so dia kat atas browser,bukan bawah b
    position: fixed;//supaya dia follow mana center position browser kita,kalau static--dia akan ada satu tmpat tak berubah2
    top: 0;//jarak dari atas
    left: 0;//jarak dari kiri
    padding: 90px 10px 10px; //jarak dari atas,kanan,bawah
    width: calc(100%); //100% view lebar
    height: calc(100%);//100% view tinggi -- meaning full skrin
    background:black;

    .detail-view {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      position: relative; //position dia stay
      width: 100%;
      max-width: 510px;
      padding: 50px 0 0;
      background-color: #fff;
      border-radius: 5px;

      h2 {
        text-transform: bold;
      }

      .data {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column;
        width: 100%;
        margin-bottom: 40px;

        .property {
          width: 90%;
          max-width: 400px;
          border-bottom: 1px solid #ccc;
          margin-bottom: 10px;

          .left { 
            float: left; 
            background-color: yellow;
          } //left class stay left
          .right { 
            float: right;
            background-color: yellow;
          } //make sure yg right class dia stay paling kanan
        }

        h3 {
          width: 90%;
          max-width: 400px;
          border-bottom: 1px solid #ccc;
        }

        .types, .abilities {
          display: center;
          justify-content: flex-start;
          flex-wrap: wrap;
          width: 90%;
          max-width: 400px;

          .type, .ability {//sekalikan
            margin: 0 10px 10px 0;
            padding: 5px 10px;
            border-radius: 20px;
            color:black;
            font-size: 1rem;
            letter-spacing: 2px;//jarak antara huruf
            text-transform: capitalize;
            word-wrap: none;

          }
        }
      }

      .close {//button close
        border-radius: 5px;
        background-color: blueviolet;
        color: #efefef;
        padding: 10px 20px;
        margin-bottom: 20px;
        font-size: 1.2rem;
        cursor: crosshair;
      }
    }

    i {
      font-size: 2rem;
      color: #efefef;
    }
  }//since semua under detail,so kita masukkan under curly .detail
</style>
