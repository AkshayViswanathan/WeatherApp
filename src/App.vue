<template>
  
      <div class="max-h-screen" >
          <div  >
            <div class="max-h-screen ">
              <img v-if="datafetch?.weather[0]?.main === 'Clouds'" :src="clouds" alt="" class="bg-cover bg-center 	" />
          <img v-else-if="datafetch?.weather[0]?.main === 'Rain'" :src="rain" alt="" class="max-h-screen	" />
          <img v-else-if="datafetch?.weather[0]?.main === 'Clear'" :src="clear" alt="" class="max-h-screen" />
          <img v-else-if="datafetch?.weather[0]?.main === 'Mist'" :src="mist" alt="" class="max-h-screen" />
          <img v-else-if="datafetch?.weather[0]?.main === 'Haze'" :src="haze" alt="" class="max-h-screen" />
          <img v-else-if="datafetch?.weather[0]?.main === 'Snow'" :src="snow" alt="" class="max-h-screen" />
          <img v-else :src="defaultimage" alt="" class="bg-contain	" />
            </div>
            <div class="absolute top-[100px] left-[100px] z-4">
              <p class="text-black ">{{ rounoffDegreeCelcus }} °C</p>
                <p class="text-red-500">Location: {{ datafetch?.name }}, {{ datafetch?.sys?.country }}</p>
                <p>{{ Todysdate }} </p> 

            </div>
           </div>
  
   
  <div class="absolute top-0 right-0 w-[40%] h-[60vh] bg-slate-800 p-8 bg-gray-500	">
     
    
     <input class="seachbox rounded-md items-starts my-8 ml-8 mr-2 p-2"  type="text" v-model="searchTerm" placeholder="search..." />
     <i class="fa-solid fa-magnifying-glass text-white	hover:bg-gray-600 text-xl" @click="handleSubmit"></i>

     <p class="text-white align-center">Weather Details...</p>


     <div class="flex justify-around">
        <label class="text-white	" >Temp_min</label>
        <p class="text-white	">{{ datafetch?.main?.temp_min }}</p>
     </div>
      
     <div class="flex justify-around">
          <label for="tempMax" class="text-white	">Temp_max</label>
          <p class="text-white	">{{ datafetch?.main?.temp_max }}</p>
      </div>

     <div class="flex justify-around" >
         <p class="text-white	"> {{ datafetch?.weather[0]?.main }}</p>
         <label class="text-white	"> {{ datafetch?.clouds?.all }}% </label>
     </div>
     
      <div class="flex justify-around">
        <label class="text-white	" >Humidity</label>
        <p class="text-white	">{{ datafetch?.main?.humidity}}</p>
      </div >
      <div class="flex 	justify-around">
        <label class="text-white	" > Wind </label>
        <p class="text-white	"> {{ datafetch?.wind?.speed}} </p>

      </div>
    
 </div>
  </div>
  <p>{{ datafetch }}</p>

  
</template>




<script setup>
import { ref, watch } from "vue";
import axios from "axios";
import mist from "./assets/mist.jpg"
import snow from "./assets/snow.jpeg"
import rain from "./assets/rain2.jpg" 
import clear from "./assets/clear.jpg" 
import clouds from "./assets/Clouds.jpg"
import defaultimage from "./assets/default.jpg"


const datafetch = ref(null)
const searchTerm = ref(null)
const rounoffDegreeCelcus = ref(null)


const Todysdate = new Date().toLocaleDateString('en-us', {
  weekdays: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric'
  
})



async function handleSubmit(){
  try{
  const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${searchTerm.value}&units=metric&appid=9a07fd6e72eba80e9494d4624edf23f9`;
  const data = await axios.get(apiUrl).then(response => response.data)
  datafetch.value = data

}
catch(error){ 
  console.error("fetch faild" )
 
}
}


watch(datafetch, ()=>{
  rounoffDegreeCelcus.value= Math.round(datafetch?.value?.main?.temp)
})

</script>

<style  scoped>



</style>