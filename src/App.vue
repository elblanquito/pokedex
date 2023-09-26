<script setup>
import { ref } from 'vue';
import axios from 'axios'

async function cargar(){
  let todo = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=50')
  
  
  for (let i = 0; i < todo.data.results.length; i++){
    let r = await axios.get(todo.data.results[i].url)
    data.value.push({
      name: r.data.name.charAt(0).toUpperCase() + r.data.name.slice(1),
      img: r.data.sprites.other['official-artwork'].front_default
    })
  }
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
}

  

function reset_tarjeta() {
  mostrartarjeta.value = false;
  imageUrl.value = './src/assets/cargando.gif';
  nombre.value = '- - - - -';
  peso.value = '- - -';
  altura.value = '- - -';
  tipo.value = '';
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
let mostrartarjeta = ref(false);
let imageUrl = ref();
let nombre = ref();
let peso = ref();
let altura = ref();
let tipo = ref();
let estadisticas = ref([]);
reset_tarjeta()


let numeros = [];

for (let i = 1; i <= 50; i++) {
  numeros.push(i);
}





</script>

<template>
  <div class="cont">
    <button @click="obtener('https://pokeapi.co/api/v2/pokemon/4/',true)" class="obtener">obtener info</button>
    <div class="pokecont">
      <div v-for="dato in data" :key="dato.id" class="poke">
        {{ dato.name }}
        <img :src="dato.img" :alt="dato.name" class="pokeimg">
      </div>
    
    </div>
    <div class="conttarjeta" v-if="mostrartarjeta">
      <div class="nombrepoke txtpoke">{{ nombre }}</div>
      <img :src="imageUrl" class="imagenpoke">
      <div class="datoscont">
        <div class="dato">altura: {{ altura }}</div>
        <div class="dato">peso: {{ peso }}</div>
      </div>
      <div class="estadisticascont">
        <div v-for="estadistica in estadisticas" :key="estadistica.id" class="estadistica" >
          {{ estadistica.stat.name }}:
          <div class="barracont">
            <div class="barra" :style="{ width: estadistica.base_stat + '%' }">
              {{ estadistica.base_stat }}
            </div>
          </div>
        </div>
      </div>
      <button @click="reset_tarjeta()" class="cerrabtn">cerrar</button>
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

.cont {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;

  color-scheme: light dark;
  color: rgb(0, 0, 0);
  background-color: #ffffff;
  min-height: 100vh;
  display: grid;
  justify-content: center;
  
}

.pokecont{
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  width: 90vw;
  border: solid 1px black;
  display: grid;
  /* Columnas automáticas, cada una con mínimo 100px de ancho */
  grid-template-columns: repeat(auto-fill, minmax(1px, 250px)); 
  justify-content: center;
}

.poke{
  justify-content: center;
  display: grid;
  width: calc(100% - 40px);
  height: ;
  margin: 10px;
  border: solid 1px black;
  padding: 10px;
}

.pokeimg{
  width: 60%;
  margin: auto;
}





.obtener {
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 15px 20px;
  font-size: 20px;
  margin-top: 20px;
  font-weight: 500;
  font-family: inherit;
  background-color: #1a1a1a;
  cursor: pointer;
  transition: border-color 0.25s;

  
  margin-left: auto;
  margin-right: auto;
  width: fit-content;
  height: fit-content;
}
.obtener:hover {
  border-color: #646cff;
}
.obtener:focus,
.obtener:focus-visible {
  outline: 4px auto -webkit-focus-ring-color;
}




@media (prefers-color-scheme: light) {
  :root {
    color: #213547;
    background-color: #ffffff;
  }
  a:hover {
    color: #747bff;
  }
  .obtener {
    background-color: #f9f9f9;
  }
}



.conttarjeta {
  position: absolute;
  margin: 20px;
  border-radius: 20px;
  border: solid 5px rgba(255, 255, 255, 0.63);
  background-color: #2f6125; 
  width: 90vw;
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
  border: solid 5px #24180e;
  background-color: antiquewhite;
}

.datoscont{
  margin: 10px 10%;
  display: grid;
  grid-template-columns: 50% 50%;
}

.dato{

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

.estadisticascont{
  width: 80%;
  margin: 20px;
  margin-left: auto;
  margin-right: auto;
  border-radius: 20px;
  padding: 20px;
  border: solid 4px rgba(255, 255, 255, 0.685);

  font-size: large;
  font-weight: bold;
  background-color: #000000;
  transition: width 0.5s, height 0.5s;
}

.estadistica{
  margin: 15px 0px;
}

.barracont{
  border: solid 5px rgba(114, 114, 114, 0);
  border-radius: 0px;
  width: auto;
  overflow: hidden;
  background-color: #700000;
}

.barra{
  height: 100%;
  width: 10%;
  background-color: rgb(255, 255, 0);
  border-radius: 5px;
  color: #5f3e00;
  text-align: center;
  transition: width 0.5s
}

.cerrabtn{
  display: grid;
  background-color: #970000;
  margin: 20px auto;
}

</style>
