<template>
    <section id="location" >     

        <!-- City list start -->
        <div v-if =  'isShow && !cityDetails' >
            <!-- Refresh button-->
            <div class="arrange-btn">
                 <button @click='refresh()' class="btn2">
                    <i class="fas fa-sync"></i>
                </button>

                <button class="btn" @click='openAddLocationComponent'>ADD CITY</button>
            </div>   
            <p v-if='!area.length'> No city Added yet</p>
            <div v-if='area.length' class= 'card-holder'>
                <div  v-for='city in area' :key='city.id' >

                    <div @click ='showCityWeatherDetails(city.name) ' class= 'cards fx'>
                    {{city.name}} <span>{{Math.round(city.main.temp)}}&deg;C</span>
                    </div>

                </div>
            </div>

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
        citiesIds: ['2332453', '3196359'],
        myCities: [],
        url : 'http://api.openweathermap.org/data/2.5/',
        key : 'f499a2f595cddaac83d35435ce89a26e',
        cityDetails : false,
        cityName : '',
        area : []

        }
    },
    
    mounted () {
        if(localStorage.area){
        this.area = JSON.parse(localStorage.area)
        console.log('Found')
        }else{
        console.log('not found')
        localStorage.area = JSON.stringify([])
        }
        this.callApi();
        
    },
    
    
    methods: {
        
        openAddLocationComponent : function (){
        this.isShow = !this.isShow
        },

        callApi :  function (){
        console.log(this.area)

        var results = [];
            for(var i=0; i<this.area.length; i++) {
            
            results.push(this.area[i].id)
            
            }
        
        fetch (`${this.url}group?id=${results}&appid=${this.key}&units=metric`)
        .then(response => response.json())
        .then(data => {
            this.area = data.list       
            });
        },
    
        showCityWeatherDetails : function(cityName){
        this.cityDetails = true;
        this.cityName = cityName
        
        },

        close : function (){
        this.cityDetails = false;
        this.isShow = true;
        },

        refresh : function(){
        this.callApi();
        
        },
    
      }
    
    }

</script>
