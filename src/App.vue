<template>
  <div>
      <div class="w-[90vw] h-[70vh] relative mx-auto flex " >
          <div class="p-0 w-[90vw] h-[60vh]  relative" >
            <div class="-z-10 w-[90vw] h-[60vh] object-contain">
                <img :src="{ backgroundImage }" alt=""  class="w-full h-full"/>
            </div>
            <div class="absolute top-[100px] left-[100px] z-4">
              <p class="text-black">{{ rounoff }} °C</p>
                <p class="text-red-500">Location: {{ datafetch?.name }}</p>
                <p>{{ Todysdate }} </p> 

            </div>
           </div>
  
   
  <div class="absolute top-0 right-0 w-[40%] h-[60vh] bg-slate-800">
     
     <i class="fa-solid fa-magnifying-glass">
         
     </i>
     <input class="seachbox" type="text" v-model="searchTerm" placeholder="search..." />
     <p>{{ datafetch }}</p>

     <div class="flex justify-around" >div
         <p> {{ datafetch?.weather[0]?.main }}</p>
         <label > {{ datafetch?.clouds?.all }}% </label>
     </div>
     
     <div class="flex justify-around">
        <label >Temp_min</label>
        <pdiv>{{ datafetch?.main?.temp_min }}</pdiv>
     </div>
      
     <div class="flex justify-around">
          <label for="tempMax">Temp_max</label>
          <!-- <p >{{ datafetch?.main?.temp_max }}</p> -->
      </div>
  
    
     <button class="bg-white-500 hover:bg-blue-700 text-white font-bold py-2 px-4" @click="handleSubmit">search</button>
 </div>
  </div>

  </div>
</template>




<script setup>
import { ref, computed, watch } from "vue";
import axios from "axios";
import Sunny from "./assets/sunny.jpeg"
import snow from "./assets/snow.jpeg"
import foggy from "./assets/foggy.jpeg"
import defaultimage from "./assets/default.jpeg"



const datafetch = ref(null)
const searchTerm = ref(null)
const rounoff = ref(null)


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

const backgroundImage = computed(() => {
  const temp = datafetch.value?.main?.temp;

  if (temp > 25 && temp < 35) {
    return Sunny;
  } else if (temp > 10 && temp < 24) {
    return foggy;
  } else if (temp > -25 && temp < 10) {
    return snow;
  } else {
    return defaultimage;
  }
});
watch(datafetch, () => {
  backgroundImage.value;
});

watch(datafetch, ()=>{
  rounoff.value= Math.round(datafetch?.value?.main?.temp)
})

</script>

<style  scoped>



</style>