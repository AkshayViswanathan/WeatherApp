<template>
  <div class="container">
{{ date }}


  </div>

  <div >
     
      <i class="fa-solid fa-magnifying-glass">
          
      </i>
      <input type="text" v-model="searchTerm" />
      <p>{{ datafetch }}</p>
      <p>{{ datafetch.name }}</p>
     
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



</style>