<template >
    <div class="container">
        <div>
            <div class="search-label">Search for city</div>
            <input type="text" name="input" @keyup="debounceSearch" ref="input">
        </div>
        <div>
            <WeatherInfoItem name="Temperature" :value="temp"></WeatherInfoItem>
            <WeatherInfoItem name="Feels like" :value="feelsLike"></WeatherInfoItem>
            <WeatherInfoItem name="Minimum temperature" :value="tempMin"></WeatherInfoItem>
            <WeatherInfoItem name="Maximum temperature" :value="tempMax"></WeatherInfoItem>
            <WeatherInfoItem name="Humidity" :value="humidity"></WeatherInfoItem>
        </div>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';
import WeatherInfoItem from '@/components/WeatherInfoItem.vue';

export default defineComponent({
    components: {
        WeatherInfoItem
    },
    data() {
        return {
            temp: -1,
            feelsLike: -1,
            tempMin: -1,
            tempMax: -1,
            humidity: -1,
            debounceSearch: function(){},
        }
    },
    created() {
        this.debounceSearch = this.debounce(() => this.search(), 500);
    },
    methods: {
        debounce(func: Function, timeout = 300) {
            let timer: number;
            return () => {
                clearTimeout(timer);
                timer = setTimeout(() => { func(); }, timeout);
            }
        },
        async search() {
            const inputElement = this.$refs.input as HTMLInputElement;
            const city = inputElement.value;
            try {
                // Info about city received
                const res = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${import.meta.env.VITE_WEATHER_API}`);
                this.temp = res.data.main.temp;
                this.feelsLike = res.data.main.feels_like;
                this.tempMin = res.data.main.temp_min;
                this.tempMax = res.data.main.temp_max;
                this.humidity = res.data.main.humidity;
            } catch (e) {
                // Invalid city
                console.log(e);
            }
        },
        fahrenheitToCelsius(fahrenheit: number) {
            return (fahrenheit - 32) / 1.8
        }
    }
});
</script>
<style scoped>
    .search-label {
        text-align: center;
    }

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