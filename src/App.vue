<script setup>
import { ref } from 'vue';
import axios from 'axios'

async function cargar(){
  let todo = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=50')
  
  
  for (let i = 0; i < todo.data.results.length; i++){
    let r = await axios.get(todo.data.results[i].url)
    let typesArray = [];
        for (let j = 0; j < r.data.types.length; j++) {
          typesArray.push(r.data.types[j].type.name);
        }

    data.value.push({
      name: r.data.name.charAt(0).toUpperCase() + r.data.name.slice(1),
      img: r.data.sprites.other['official-artwork'].front_default,
      url: todo.data.results[i].url,
      types: typesArray
    })
  }
  console.log(data.value)
}



cargar()

async function obtener(url){
  /*
  let r = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=50&offset=0')
  console.log(r)
  */
  let r = await axios.get(url)
  console.log(r)
  mostrartarjeta.value = true
  imageUrl.value = r.data.sprites.other['official-artwork'].front_default
  nombre.value = r.data.name.charAt(0).toUpperCase() + r.data.name.slice(1)
  estadisticas.value = r.data.stats
  peso.value = r.data.weight
  altura.value = r.data.height
  tipos.value = []
  for (let i = 0; i < r.data.types.length; i++) {
    tipos.value.push(r.data.types[i].type.name);
  }
}

  

function reset_tarjeta() {
  mostrartarjeta.value = false;
  imageUrl.value = './src/assets/cargando.gif';
  nombre.value = '- - - - -';
  peso.value = '- - -';
  altura.value = '- - -';
  tipos.value = ['- - -']
  estadisticas.value = [
    {base_stat: 10, stat: {name: "hp"}},
    {base_stat: 10, stat: {name: "attack"}},
    {base_stat: 10, stat: {name: "defense"}},
    {base_stat: 10, stat: {name: "special-attack"}},
    {base_stat: 10, stat: {name: "special-defense"}},
    {base_stat: 10, stat: {name: "speed"}}
  ];
}

let data = ref([])
let mostrartarjeta = ref(true);
let imageUrl = ref();
let nombre = ref();
let peso = ref();
let altura = ref();
let tipos = ref();
let estadisticas = ref([]);
reset_tarjeta()


let numeros = [];

for (let i = 1; i <= 50; i++) {
  numeros.push(i);
}








</script>

<template>
  <div>
    <q-toolbar class="bg-accent text-white head fixed-top">
      <q-input  class="bg-white buscartxt"  v-model = "texto">
        <q-btn flat round dense icon="search" />
      </q-input>




     

      <q-btn-dropdown stretch flat label="Filtros">
        <q-list>
          <q-item-label header>Clases de pokemons</q-item-label>
          <q-separator inset spaced />

          <q-item v-for="n in 3" :key="`x.${n}`" clickable v-close-popup tabindex="0">

            <q-item-section avatar>
              <q-avatar icon="plant" color="secondary" text-color="white" />
            </q-item-section>

            <q-item-section>
              <q-item-label>fuego</q-item-label>
              <q-item-label caption>February 22, 2016</q-item-label>
            </q-item-section>
            
            <q-item-section side>
              <q-icon name="info" />
            </q-item-section>
          </q-item>

          <q-separator inset spaced />
        </q-list>
      </q-btn-dropdown>
    </q-toolbar>
  <div class="cont">
    <div class="pokecont">
      <button v-for="dato in data" :key="dato.id" class="poke" @click="obtener(dato.url)">
        {{ dato.name }}
        <img :src="dato.img" :alt="dato.name" class="pokeimg">
        <div>
          <button v-for="tipo in dato.types" :key="tipo.id" class="type" :class="[tipo]">
            {{ tipo }}
          </button>
        </div>
      </button>
      
    
    </div>
    <div class="conttarjeta" v-if="true">
      <div class="nombrepoke txtpoke">{{ nombre }}</div>
      <img :src="imageUrl" class="imagenpoke">
      <div class="datoscont">
        <p class="dato"><div>altura</div>{{ altura }}</p>
        <p class="dato"><div>peso</div> {{ peso }}</p>
        <button v-for="tipo in tipos" :key="tipo.id" class="type2 type" :class="[tipo]">
        {{ tipo }}
        </button>
      </div>
      <div class="type2cont">
          
      </div>
      <div class="estadisticascont">
        <div v-for="estadistica in estadisticas" :key="estadistica.id" class="estadistica" >
          <p class="estadisticatxt">{{ estadistica.stat.name }}</p>
          <button class="barracont">
            <div class="barra" 
            :style="{ width: estadistica.base_stat > 100 ? '100%' : estadistica.base_stat + '%' }">
              {{ estadistica.base_stat }}
            </div>
          </button>
        </div>
      </div>
      <!-- <button @click="reset_tarjeta()" class="cerrabtn">cerrar</button>
 -->    </div>
  </div>
</div>

</template>

<style scoped>
*{
  margin: 0px;
  padding: 0px;
}

@keyframes aparecer{
  0% {
    opacity: 0;
    transform: translate(0px, 100px);
  }
  100% {
    opacity: 1;
  }
}

.head{
  padding: 0px;
  z-index: 3;
}

.buscartxt{
  width: 350px;
  padding: 0px 20px;
}

.cont {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
  display: grid;
  grid-template-columns: auto 400px;
  color-scheme: light dark;
  color: white;
  background-color: #000000;
  min-height: 100vh;
  display: grid;
  justify-content: center;
  padding-top: 40px;
}

.pokecont{
  color: white;
  padding: 20px;
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  width: 100%;
  border: solid 1px black;
  background-color: #000000;
  display: flex;
  flex-wrap: wrap; /* Permite que los elementos se envuelvan en filas */
  justify-content: center; /* Distribuye el espacio entre los elementos */
  box-sizing: border-box;
}


.poke{
  color: white;
  display: grid;
  background-color: black;
  width: 250px;
  margin:20px 10px;
  border: solid 2px rgb(255, 255, 255);
  padding: 10px;
  font-size: 20px;
  transition: transform 0.5s ease; 
}

.poke:hover{
  border: solid 2px yellow;
  color: yellow;
  box-shadow: 0px 0px 10px yellow;
}

.poke:hover .pokeimg {
  border: solid 3px yellow;
}



.pokeimg{
  width: 60%;
  margin: 10px auto;
  background: #d4d4c5;
  border: solid 3px rgb(255, 255, 255);
}


.type{
  color: white;
  background-color: rgb(167, 167, 167);
  width:fit-content;
  padding: 5px 7px;
  margin: 10px;
  border: none;
  border-radius: 5px;
  font-weight: bold;
}

.grass{
  background-color: rgb(32, 160, 0);
}
.poison{
  background-color: rgb(83, 0, 160);
}

.fire{
  background-color: rgb(233, 110, 28);
}
.flying{
  background-color: rgb(69, 167, 154);
}
.water{
  background-color: rgb(50, 66, 156);
}
.bug{
  background-color: rgb(111, 145, 58);
}
.electric{
  background-color: rgb(209, 160, 0);
}
.ground{
  background-color: rgb(133, 85, 46);
}
.fairy{
  background-color: rgb(207, 0, 121);
}



.conttarjeta {
  position: sticky;
  top: calc(50% + 50px);
  transform: translate(0,-50%);
  color: white;
  margin: auto;
  margin-top: 0;
  border-radius: 20px;
  border: solid 4px rgba(255, 255, 255);
  background-color: #000000; 
  width: 90%;
  max-width: 500px;
  min-width: 200px;
  animation: aparecer 0.2s ease-out ;
}

.imagenpoke{
  display: grid;
  width: 80%;
  max-width: 300px;
  margin-left: auto;
  margin-right: auto;
  border: solid 5px #ffffff;
  background-color: antiquewhite;
}

.datoscont{
  margin: 10px 10%;
  display: grid;
  grid-template-columns: 25% 25% auto auto;
  justify-content: center;
  align-items: center;
}

.dato{
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  margin: auto;
}

.nombrepoke{
  text-align: center;
  font-size: 40px;
  font-weight: bold;
  margin-top: 10px;
}

.type2cont{
  width: fit-content;
  margin: auto ;
}

.type2{
  color: white;
  width:fit-content;
  padding: 10px 15px;
  margin: 0px;
  border: none;
  border-radius: 5px;
  font-size: 15px;
  font-weight: bold;
}

.estadisticascont{

  width: 90%;
  margin: auto;
  margin-bottom: 10px;
  font-size: large;
  font-weight: bold;
  text-align:left;
  background-color: #000000;
}

.estadistica{
  margin: 10px 0px;
  display: grid;
  grid-template-columns: 45% auto;

  align-items: center;
}

.estadisticatxt{
  text-align: end;
  margin-right: 15px;
}

.barracont{
  border: none;
  border-radius: 0px;
  width: 100%;
  margin: right;
  overflow: hidden;
  background-color: #700000;
}

.barra{
  height: 100%;
  width: 10%;
  display: grid;
  align-items: center;
  background-color: rgb(255, 255, 0);
  border-radius: 5px;
  color: #5f3e00;
  text-align: center;
  transition: width 1s;
}

.cerrabtn{
  color: white;
  font-size: 17px;
  font-weight: bold;
  display: grid;
  background-color: #000000;
  margin: 20px auto;
  padding: 10px 45px;
  border: solid white;
  border-radius: 10px;
}

.cerrabtn:hover{
  border: solid yellow;
  color: yellow;
}

</style>
