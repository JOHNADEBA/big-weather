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
  name: 'add-locations',
  components: {
    CallApi
  },
  data () {
    return {
      cityName:'',
      isShowThird : false,
      url : 'http://api.openweathermap.org/data/2.5/',
      key : 'f499a2f595cddaac83d35435ce89a26e',
      cityId : [],

      idCity : '',
      area: []
    }
  },
  methods: {
    openCallApiComponent : function (){
       if(this.cityName.length > 0){
         
         fetch (`${this.url}weather?q=${this.cityName}&appid=${this.key}&units=metric`)
        .then(response => response.json())
        .then(data => {
          this.save(data.id)
          });

        this.isShowThird = true
    
        }
    },

  save: function(id){
    console.log('id save === ' + this.idCity)

    if(JSON.parse(localStorage.area ).length !== 0){
      this.area = JSON.parse (localStorage.area);
      this.area.push({
            id : id,
            name : this.cityName
    
            });
    } else {
      this.area.push({
            id : id,
           name : this.cityName
    
         });
    }
    
     localStorage.area = JSON.stringify(this.area)
  },

    getCityIdFromApi : function (){
      fetch (`${this.url}weather?q=${this.cityName}&appid=${this.key}&units=metric`)
     .then(response => response.json())
     .then(data => {
       return data.id
       });
    },
          
  }
}

</script>

