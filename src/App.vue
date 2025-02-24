<!--=================================================Script=================================================-->
<script>
import axios from 'axios'

export default {
    data() {
        return {
            city: "",
            error: "",
            info: null,
        }
    },
    computed: {
        cityName(){
            return "«" + this.city + "»";
        },
        showTemp(){
            return "Температура: " + this.info.main.temp;
        },
        showFeelsLike(){
            return "Ощущается как: " + this.info.main.feels_like;
        },
        showClouds(){
            return this.info.weather[0].icon;
        },
    },
    methods: {
        getWeather() {
            if(this.city.trim().length < 2){
                this.error= "Название должно содержать более одного символа!"
                return false;
            }
            this.error= "";
            
            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=e710110bffd69fd169bee812851e14ff`)
                .then(res => (this.info = res.data))
        }
    }
}
</script>

<!--================================================Template================================================-->
<template>
    <div class="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{ city === "" ? "Вашем городе" : cityName }}</p>
        <input type="text" v-model="city" placeholder="Введите город">
        <button v-if="city" v-on:click="getWeather()">Получить погоду</button>
        <button disabled v-else>Введите название города</button>
        <p class="error">{{ error }}</p>
        
        <div v-if="info">
            <p>{{ showTemp }}</p>
            <p>{{ showFeelsLike }}</p>
            <img :src="`https://openweathermap.org/img/wn/${showClouds}@2x.png`" alt="weather-icon"/>
        </div>
    </div>
</template>

<!--=================================================Style==================================================-->
<style scoped>
.error {
    color: #d03939;
}
.wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: #1f0f24;
    text-align: center;
    color: white;
}

.wrapper h1 {
    margin-top: 50px
}

.wrapper p {
    margin-top: 20px;
}

.wrapper input {
    margin-top: 30px;
    background: #3e2047;
    border: none;
    border-bottom: 2px solid #110813;
    border-radius: 5px;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}

.wrapper input:focus {
    border-bottom-color: #9430af;
    transition: border-botton-color, 1000ms;
}

.wrapper button {
    background: rgb(34, 193, 195, 1);
    color: #ffffff;
    border-radius: 10px;
    border: 2px solid rgb(0, 248, 255);
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
}

.wrapper button:disabled {
    background: rgb(19, 90, 94);
    cursor: not-allowed;
}

.wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
}
</style>
