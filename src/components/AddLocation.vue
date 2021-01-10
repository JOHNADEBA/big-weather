<template>
  <section id="addlocation">
     
    <form v-if ='!isShowThird' class="search" @submit.prevent="openCallApiComponent" >
        <div>
            <input v-model="cityName" type="text" placeholder="e.g Ljubljana">
        </div>
        <!-- action buttons -->
        <button  @click="$emit('hide-modal')">Cancel</button>
        <button type="submit">Finish</button>
    </form>

    <div v-else>
        <CallApi  :cityName="cityName" />
    </div>

  </section>
</template>

<script>
    import CallApi from './CallApi'
    export default {
    name: 'addlocation',
    components: {
    CallApi
  },
  props: ['cities', 'isShow', 'search-city'],
  data () {
    return {
      cityName:'',
      isShowThird : false,
      url : 'http://api.openweathermap.org/data/2.5/',
      key : 'f499a2f595cddaac83d35435ce89a26e',
      cityId : []
    }
  },
  watch: {
    cityName(newcity){
      localStorage.cityname = JSON.stringify(newcity)
    }
  },
  mounted () {
    if(localStorage.cityName){
        this.cityName = JSON.parse (localStorage.cityname)
    }
  },
  methods: {
    openCallApiComponent : function (){
       if(this.cityName.length > 0){
            this.isShowThird = true    
            }    
        }
    }
  }
</script>

<style >


</style>