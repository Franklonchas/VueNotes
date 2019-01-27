<template>
    <div id="app">
        <div id="weather">
            <span>Tiempo actual: {{overcast}}</span><br>
            <h1>{{name}}</h1>
            <span class="temperature">Temperatura: {{currentTemp}}°</span><br>
            <span id="temp-values">Min {{minTemp}}° <br> Max {{maxTemp}}°</span>
        </div>
        <div id="info">
            <span>Hora del amanecer: {{sunrise}}</span><br>
            <span>Hora de la puesta de sol: {{sunset}}</span><br>
            <span>Humedad: {{humidity}}</span><br>
            <span>Presión: {{pressure}}</span><br>
            <span>Viento: {{wind}}</span><br>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    const API = 'http://api.openweathermap.org/data/2.5/weather?units=metric';
    const KEY = '&APPID=dfe44a7ee342e2aa8a08f9d3d263e48d';

    export default {
        name: "VueWeather",
        data: function () {
            return {
                currentTemp: '',
                name: '',
                minTemp: '',
                maxTemp: '',
                sunrise: '',
                sunset: '',
                pressure: '',
                humidity: '',
                wind: '',
                overcast: '',
                location: ''
            }
        },
        methods: {
            getWeather(url) {
                axios.get(url)
                    .then(response => {
                        this.name = response.data.name;
                        this.currentTemp = response.data.main.temp;
                        this.minTemp = response.data.main.temp_min;
                        this.maxTemp = response.data.main.temp_max;
                        this.pressure = response.data.main.pressure;
                        this.humidity = response.data.main.humidity + '%';
                        this.wind = response.data.wind.speed + 'm/s';
                        this.overcast = response.data.weather[0].description;
                        this.sunrise = new Date(response.data.sys.sunrise * 1000).toLocaleTimeString("en-GB").slice(0, 4);
                        this.sunset = new Date(response.data.sys.sunset * 1000).toLocaleTimeString("en-GB").slice(0, 4);
                    })
                    .catch(error => {
                        console.log(error);
                    });
            },
            geolocation() {
                navigator.geolocation.getCurrentPosition(this.buildUrl, this.geoError);
            },
            buildUrl(position) {
                const lat = position.coords.latitude;
                const lon = position.coords.longitude;

                this.getWeather(API + '&lat=' + lat + '&lon=' + lon + KEY);
            },
            geoError(error) {
                this.getWeather(API + '&lat=0&lon=0' + KEY);
            }
        },
        beforeMount() {
            this.geolocation();
        },
    }

</script>

<style scoped>

    #app {
        width: 520px;
        height: 170px;
        position: absolute;
        top: 35%;
        left: 35%;
        color: white;
    }

    #weather {
        padding: 15px;
        vertical-align: middle;
    }

    .temperature {
        font-size: 40px;
        vertical-align: top;
        position: absolute;
        left: 80px;
    }

    #temp-values {
        text-align: right;
        text-justify: distribute;
        display: block;
        position: relative;
        top: -60px;
    }

    #info {
        padding-left: 20px;
        position: relative;
        top: -20px;
    }
</style>