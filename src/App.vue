<template>
  
      <div class=" h-screen overflow-hidden"  >
          <div class="effect h-screen overflow-hidden" >
            <div class=" h-screen w-full ">

              <img :src="imageChange" alt="" class="w-[100%] h-full object-cover"/>

            </div>
            <div class="absolute top-[100px] left-[100px] z-4">
              <p class="text-black text-6xl text-white">{{ rounoffDegreeCelcus }} °C</p>
          
              <div class="flex gap-5">

                <p class="text-white	text-5xl "> {{ datafetch?.weather[0]?.main }}</p>
                <i v-if="datafetch?.weather[0]?.main === 'Clouds'" class="fa-solid fa-cloud text-white	text-3xl pt-.5"></i>
                <i v-else-if="datafetch?.weather[0]?.main === 'Rain'" class="fa-solid fa-cloud-rain text-white	text-3xl pt-.5"></i>
                <i v-else-if="datafetch?.weather[0]?.main === 'Snow'" class="fa-regular fa-snowflake text-white	text-3xl pt-.5"></i>
                <i  v-else-if="datafetch?.weather[0]?.main === 'Mist'"  class="fa-solid fa-smog text-white	text-3xl pt-.5"></i>
                <i  v-else-if="datafetch?.weather[0]?.main === 'haze'"  class="fa-solid fa-smog text-white	text-3xl pt-.5"></i>
                <i v-else :src="defaultimage"   class="fa-solid fa-earth-americas text-white	text-3xl pt-.5"></i>
              </div>

                <p class="text-red-500 text-4xl">Location: {{ datafetch?.name }}, {{ datafetch?.sys?.country }}</p>
                <p class="text-3xl text-white">{{ Todysdate }} </p> 

            </div>
           </div>
  
   
  <div class="glassmorphism absolute top-0 right-0 w-[40%] h-[100vh] bg-slate-800 p-8 bg-gray-500	">
     
    
     <input class="glassmorphism text-white seachbox rounded-md items-starts my-8 ml-8 mr-2 p-2"  type="text" v-model="searchTerm" placeholder="search..." />
     <i class="fa-solid fa-magnifying-glass text-white	hover:bg-gray-600 text-3xl pt-.5" @click="handleSubmit"></i>
       
     <div v-if="datafetch" class="flex flex-col gap-8 mx-10">

  
     <p class="text-white align-center">Weather Details...</p>
     <p class=" text-white uppercase">{{ datafetch?.weather[0]?.description }}</p>


     <div class="flex justify-between">
        <label class="text-white	" >Temp_min</label>
        <p class="text-white">{{ datafetch?.main?.temp_min }}</p>
     </div>
      
     <div class="flex justify-between">
          <label for="tempMax" class="text-white	">Temp_max</label>
          <p class="text-white	">{{ datafetch?.main?.temp_max }}</p>
      </div>

     <div class="flex justify-between" >
         <p class="text-white	">clouds</p>
         <label class="text-white	"> {{ datafetch?.clouds?.all }}% </label>
     </div>
     
      <div class="flex justify-between">
        <label class="text-white	" >Humidity</label>
        <p class="text-white	">{{ datafetch?.main?.humidity}}</p>
      </div >
      <div class="flex 	justify-between">
        <label class="text-white	" > Wind </label>
        <p class="text-white	"> {{ datafetch?.wind?.speed}} </p>

      </div>
         </div>
 </div>
  </div>

</template>




<script setup>
import { ref, watch , computed} from "vue";
import axios from "axios";
import mist from "./assets/mist.jpg"
import snow from "./assets/snow.jpeg"
import rain from "./assets/rain2.jpg" 
import clear from "./assets/clear.jpg" 
import clouds from "./assets/cloud2.jpg"
import haze from "./assets/haze.jpg"
import defaultimage from "./assets/default2.jpg"


const datafetch = ref(null)
const searchTerm = ref(null)
const rounoffDegreeCelcus = ref(null)
const APIKEY =import.meta.env.VITE_API_KEY;
const APIURL =import.meta.env.VITE_API_URL ;



const Todysdate = new Date().toLocaleDateString('en-us', {
  weekdays: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric'
  
})



async function handleSubmit(){
  try{
  const apiUrl = `${APIURL}?q=${searchTerm.value}&units=metric&appid=${APIKEY}`;
  const data = await axios.get(apiUrl).then(response => response.data)
  datafetch.value = data
  imageChange()
}
catch(error){ 
  console.error("fetch faild" )
}
}

const imageChange = computed(() => {
  if (datafetch.value) {
    const weatherCondition = datafetch.value.weather[0].main;
    switch (weatherCondition) {
      case 'Clouds':
        return clouds;
      case 'Rain':
        return rain;
      case 'Snow':
        return snow;
      case 'Clear':
        return clear;
      case 'Mist':
        return mist;
      case 'Haze':
        return haze;
      default:
        return defaultimage;
    }
  } else {
    // If datafetch is null or undefined, return the default image
    return defaultimage;
  }
});
watch(datafetch, ()=>{
  rounoffDegreeCelcus.value= Math.round(datafetch?.value?.main?.temp)
})

</script>

<style  scoped>



</style>