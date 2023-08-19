<template >
    <div class="container">
        <div class="button" @click="clickHandler">Show weather for Ljubljana</div>
        <div>
            <div>
                Temperature: {{ temp }}
            </div>
            <div>
                Feels like: {{ feelsLike }}
            </div>
            <div>
                Minimum temperature: {{ tempMin }}
            </div>
            <div>
                Maximum temperature: {{ tempMax }}
            </div>
            <div>
                Humidity: {{ humidity }}
            </div>
        </div>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent({
    data() {
        return {
            temp: -1,
            feelsLike: -1,
            tempMin: -1,
            tempMax: -1,
            humidity: -1
        }
    },
    methods: {
        async clickHandler() {
            const res = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=Ljubljana&units=metric&appid=${import.meta.env.VITE_WEATHER_API}`);
            console.log(res);
            this.temp = res.data.main.temp;
            this.feelsLike = res.data.main.feels_like;
            this.tempMin = res.data.main.temp_min;
            this.tempMax = res.data.main.temp_max;
            this.humidity = res.data.main.humidity;
        },
        fahrenheitToCelsius(fahrenheit: number) {
            return (fahrenheit - 32) / 1.8
        }
    }
});
</script>
<style>
    .container {
        margin-top: 20px;
        display: flex;
        justify-content: space-around;
        align-items: center;
        flex-direction: column;
        row-gap: 30px;
    }

    .button {
        display: inline-block;
        background-color: #588157;
        border-radius: 10px;
        padding: 5px 10px;
        margin: auto auto;
        cursor: pointer;
        color: white;
        transition: background .5s;
    }

    .button:hover {
        background-color: #a3b18a;
    }
</style>