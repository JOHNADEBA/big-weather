<template>
  <main id="app">
    
    <h1>GET THE WEATHER UPDATE FOR YOUR FAVOURITE CITY</h1>
     <Location />
   
  </main>
</template>

<script>

import Location from './components/Location'

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
    @import './assets/style.css'

</style>