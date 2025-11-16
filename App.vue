<template>
<div class="app">
  <HelloWorld/>
  <productCus/>
    <h1>🌤️ Simple Weather App</h1>
<hr>
    <input v-model="city" @keyup.enter="getWeather" placeholder="Enter city name..." />
    

    <button @click="getWeather">Search</button>
    
    <div v-if="weather" class="weather-box">
        <h2>{{ weather.name }}</h2>
        <p>{{ weather.weather[0].description }}</p>
        <h3>{{ Math.round(weather.main.temp) }}°C</h3>
    </div>

    <p v-if="error" class="error">{{ error }}</p>
</div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue';
export default {
  components:{
    //HelloWorld
  },
  
    data() {
        return {
            city: "",
            weather: null,
            error: "",
            apiKey: "4241ab9e904fc0dcab3b64a3c28817ea", // Replace with your OpenWeatherMap API key
           
        };
    },
    methods: {
        async getWeather() {
            if (!this.city) {
                this.error = "Please enter a city name.";
                this.weather = null;
                return;
            }
            this.error = "";
            try {
                const res = await fetch(
                    `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.apiKey}&units=metric`
                );
                const data = await res.json();
                if (data.cod === "404") {
                    this.error = "City not found!";
                    this.weather = null;
                } else {
                    this.weather = data;
                }
            } catch (err) {
                this.error = "Something went wrong.";
            }
        },
    },
  
};
</script>

<style>
.app {
    max-width: 400px;
    margin: 50px auto;
    text-align: center;
    font-family: Arial, sans-serif;
    background: #e3f2fd;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
}

input {
    padding: 8px;
    width: 70%;
    border-radius: 8px;
    border: 1px solid #ccc;
}

button {
    padding: 8px 12px;
    margin-left: 8px;
    background: #42a5f5;
    border: none;
    border-radius: 8px;
    color: white;
    cursor: pointer;
}

button:hover {
    background: #1e88e5;
}

.weather-box {
    margin-top: 20px;
    background: white;
    padding: 15px;
    border-radius: 12px;
}

.error {
    color: red;
    margin-top: 10px;
}
</style>
