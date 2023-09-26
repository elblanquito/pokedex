<script setup>
import { ref } from 'vue';
import axios from 'axios'

async function optener(){
  /*
  let r = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=50&offset=0')
  console.log(r)
  */
  let r = await axios.get('https://pokeapi.co/api/v2/pokemon/1/')
  console.log(r)
  imageUrl.value = r.data.sprites.other['official-artwork'].front_default
  nombre.value = r.data.name.charAt(0).toUpperCase() + r.data.name.slice(1)
  estadisticas.value = r.data.stats
}



let imageUrl = ref('https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExZ3h5ZHRjY213c3NrOGV3dGM1YWNrbHY2bjQ0bWVvajY2bHdpOTI1YyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3oEjI6SIIHBdRxXI40/giphy.gif')
let nombre = ref('- - - - -')
let peso = ref('- - -')
let altura = ref('- - -')
let tipo = ref('')
let estadisticas = ref([
  {base_stat: 10, stat: {name: "hp"}},
  {base_stat: 10, stat: {name: "attack"}},
  {base_stat: 10, stat: {name: "defense"}},
  {base_stat: 10, stat: {name: "special-attack"}},
  {base_stat: 10, stat: {name: "special-defense"}},
  {base_stat: 10, stat: {name: "speed"}}
])

</script>

<template>
  <div class="cont">
    <button @click="optener">optener info</button>
    <div class="conttarjeta">
      <div class="nombrepoke txtpoke">{{ nombre }}</div>
      <img :src="imageUrl" class="imagenpoke">
      <div>
        <div>altura: {{ altura }}</div>
        <div>peso: {{ peso }}</div>
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
    </div>
  </div>

</template>

<style scoped>
*{
  margin: 0px;
  padding: 0px;
}
.cont {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;

  color-scheme: light dark;
  color: rgb(255, 255, 255);
  background-color: #ffffff;
  min-height: 100vh;
  display: grid;
  justify-content: center;
  
  grid-template-rows: 100px ;
}




button {
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
button:hover {
  border-color: #646cff;
}
button:focus,
button:focus-visible {
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
  button {
    background-color: #f9f9f9;
  }
}

.conttarjeta {
  display: none;
  margin: 20px;
  border-radius: 20px;
  border: solid 5px rgba(255, 255, 255, 0.63);
  background-color: #2f6125; 
  width: 90vw;
  max-width: 500px;
  min-width: 200px;
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

</style>
