<template>
    <section id="location" >
        <!-- Refresh button-->
        <button @click='close()'>
            <i class="fas fa-sync"></i>
        </button>

        <!-- City list start -->
        <div v-if =  'isShow && !cityDetails' >
            <div class= 'card-holder'>
                <div  v-for='city in this.myCities' :key='city.id' >

                    <div @click ='showCityWeatherDetails(city.name) ' class= 'cards fx'>
                    {{city.name}} <span>{{Math.round(city.main.temp)}}&deg;C</span>
                    </div>

                </div>
            </div>

            <button class="btn" @click='openAddLocationComponent'>ADD CITY</button>

        </div>

            <!-- city list end -->

        <div  v-if='!isShow && !cityDetails' >
            <AddLocation @hide-modal="openAddLocationComponent"  />
        </div>
        
        <div v-if='cityDetails'>
            <CallApi  :cityName="cityName" />
            <button class="btn" @click='close()'>BACK</button>
        </div>           
            
    </section>
</template>

<script>

    import AddLocation from './AddLocation'
    import CallApi from './CallApi'

    export default {
        name: 'location',
        components: {
        AddLocation,
        CallApi
        },
  
        data () {
            return {
            isShow : true,
            citiesIds: ['2332453', '3173435', '3117735', '3196359'],
            myCities: [],
            url : 'http://api.openweathermap.org/data/2.5/',
            key : 'f499a2f595cddaac83d35435ce89a26e',
            cityDetails : false,
            cityName : ''

            }
        },
        computed: {

        },
        mounted () {
            let ids = this.getCitiesIds();
            this.callApi(ids);
        },
        
        methods: {
            openAddLocationComponent : function (){
            this.isShow = !this.isShow
            },

            callApi :  function (ids){
                console.log('call');
                fetch (`${this.url}group?id=${ids}&appid=${this.key}&units=metric`)
                .then(response => response.json())
                .then(data => {
                this.myCities = data.list
                console.log(data)
                
                });
            },
        
            showCityWeatherDetails : function(cityName){
            this.cityDetails = true
            this.cityName = cityName
            },

            close : function (){
            this.cityDetails = false;
            this.isShow = true;
            },

            getCitiesIds: function(){
            return this.citiesIds
            },
    }
            
}
</script>

<style >


</style>