<template>
    <div class="container">
{{ date }}


    </div>

    <div >
       
        <i class="fa-solid fa-magnifying-glass">
            
        </i>
        <input type="text" v-model="searchTerm.query" @input="handleChange" />
       
        <button ></button>
    </div>
</template>




<script setup>
import { reactive } from 'vue';



const date = new Date().toLocaleDateString('en-us', {
    weekdays: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric'
    
})


const searchTerm = reactive({
    query: "",
    debounce : null,
})

function handleChange(){

    clearTimeout(searchTerm.debounce)


 searchTerm.debounce = setTimeout(()=>{

    console.log(searchTerm.query);

}, 500)

    

}

// import { ref } from 'vue';


// const data = ref([])


// mounted() {
//     axios
//       .get('https://api.openweathermap.org/data/2.5/')
//       .then(response => (this.info = response))
//   }

</script>

<style  scoped>


/* // .container{
//     background-image: url("https://img.freepik.com/premium-photo/winter-snowflakes-snow_375001-332.jpg?size=626&ext=jpg");
// } */

</style>