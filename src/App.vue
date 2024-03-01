<template>
  <div class="container">
{{ date }}
<p>Location: {{ datafetch?.name }}</p>
<p>{{ datafetch?.main?.temp }}</p>

  </div>

  <div class="">
     
      <i class="fa-solid fa-magnifying-glass">
          
      </i>
      <input type="text" v-model="searchTerm" />
      <p>{{ datafetch }}</p>
      

      <div class="flex justify-around">
          <p> {{ datafetch?.weather[0]?.main }}</p>
          <label > {{ datafetch?.clouds?.all }}% </label>
      </div>
      
      <div class="flex justify-around">
         <label >Temp_min</label>
         <p>{{ Celcius }}</p>
      </div>
       
      <div class="flex justify-around">
           <label for="tempMax">Temp_max</label>
           <p id="tempMax">{{ datafetch?.main?.temp_max }}</p>
       </div>
   
     
      <button @click="handleSubmit">search</button>
  </div>
</template>




<script setup>
import { ref } from "vue";
import axios from "axios";

const datafetch = ref(null)
const searchTerm = ref(null)
// const clear= ref(null)

const date = new Date().toLocaleDateString('en-us', {
  weekdays: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric'
  
})


const Celcius = (datafetch?.value?.main?.temp_min -32) / 1.8;


// const handleSearch= ()=>{
// clearTimeout(clear)

//   clear.value = setTimeout(async()=>{
//   const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${searchTerm.value}&appid=9a07fd6e72eba80e9494d4624edf23f9`;
//   const data = await axios.get(apiUrl).then(response => response.data)
//   datafetch.value = data
// }, 500)

// console.log(searchTerm);
// }


async function handleSubmit(){
  try{
  const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${searchTerm.value}&appid=9a07fd6e72eba80e9494d4624edf23f9`;
  const data = await axios.get(apiUrl).then(response => response.data)
  datafetch.value = data
console.log(datafetch);}
catch(error){ 
  console.error("fetch faild" )
}
}


</script>

<style  scoped>

.searchContainer{
  background-color: rgba(124, 124, 124, 0.3);

}


</style>