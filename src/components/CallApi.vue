<template id='callapi'>

 <section class="card" >
    <div class="location">
      <div class="location-city">{{weather.name}}, {{weather.sys.country}}</div> 
    </div>
     
    <div class="tempt"> {{Math.round(weather.main.temp)}} &deg;C</div>
    <div class="humid">Humidity: {{Math.round(weather.main.humidity)}}%</div>
    <div class="desc">{{weather.weather[0].description.toUpperCase()}}</div>
      
  </section>

</template>

<script>

export default {
  name: 'App',
  props: ['cityName'],
  data (){
    return {
      url : 'http://api.openweathermap.org/data/2.5/',
      key : 'f499a2f595cddaac83d35435ce89a26e',
      weather : [],
    }
  },
  methods:{

    callApi :  function (){
      fetch (`${this.url}weather?q=${this.cityName}&appid=${this.key}&units=metric`)
     .then(response => response.json())
     .then(data => {
      this.weather = data 
          //   get old data from localstorage
          let cityList = JSON.parse(localStorage["weatherDetails"]);

          // add new entries to data from localstorage
          cityList.push({ city: data.name, temperature: data.main.temp });

          console.log("updated list", cityList);

          //   save back to localstorage
          localStorage["weatherDetails"] = JSON.stringify(cityList);
        
     });
    } 
         
  },
  mounted() {
    this.callApi(); 
  }
}
</script>
