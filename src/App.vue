<template>
  <main id="app">
    
    <h1>GET THE WEATHER UPDATE FOR YOUR FAVOURITE CITY</h1>
     <Location />
   
  </main>
</template>

<script>

import Locations from './components/Locations/index'

export default {
  name: 'app',
  data(){
    return{
    }
  },
  components: {
      Location
      
  },
  
}
</script>

<style >


</style>