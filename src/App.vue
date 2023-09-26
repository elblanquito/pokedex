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



let imageUrl = ref('')
let nombre = ref('')
let peso = ref('')
let altura = ref('')
let tipo = ref('')
let estadisticas = ref([])
</script>

<template>
  <div class="cont">
    <button @click="optener">optener info</button>
    <div class="conttarjeta">
      <div class="nombrepoke txtpoke">{{ nombre }}</div>
      <img :src="imageUrl" class="imagenpoke">
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
  color: rgba(255, 255, 255, 0.87);
  background-color: #242424;
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

  font-size: large;
  font-weight: bold;
  background-color: #5f3e00;
}

.estadistica{
  margin: 20px 0px;
}

.barracont{
  border: solid 5px rgba(255, 255, 255, 0.63);
  border-radius: 0px;
  width: 97%;
  overflow: hidden;
  background-color: #ffffff;
}

.barra{
  height: 100%;
  width: 50%;
  background-color: rgb(212, 212, 69);
  border-radius: 5px;
  color: #5f3e00;
  text-align: center;
  padding-left: px;
}

</style>
