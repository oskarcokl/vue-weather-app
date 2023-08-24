<template >
    <div class="flex flex-col items-center gap-4 mt-4">
        <div class="flex flex-col items-center">
            <div class="mx-auto text-2xl">Search for city</div>
            <input type="text" class="border" name="input" @input="debounceInputChange" ref="input">
        </div>
        <div v-if="recentCities.length > 0">
            <WeatherInfoItem name="Temperature" :value="temp + ' &deg;C'"></WeatherInfoItem>
            <WeatherInfoItem name="Feels like" :value="feelsLike + ' &deg;C'"></WeatherInfoItem>
            <WeatherInfoItem name="Minimum temperature" :value="tempMin + ' &deg;C'"></WeatherInfoItem>
            <WeatherInfoItem name="Maximum temperature" :value="tempMax + ' &deg;C'"></WeatherInfoItem>
            <WeatherInfoItem name="Humidity" :value="humidity + ' %'"></WeatherInfoItem>
        </div>
        <div v-if="recentCities.length > 0" class="flex flex-col items-center mt-3">
            <h1 class="text-2xl">Recent cities</h1>
            <div  v-for="(city, index) in recentCities" :key="index">
                <div class="cursor-pointer" @click="cityClick(city)">{{city}}</div>
            </div>
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
            debounceInputChange: function(){},
            recentCities: [] as string[],
        }
    },
    created() {
        this.debounceInputChange = this.debounce(() => this.handleInputChange(), 500);
    },
    methods: {
        cityClick(city: string) {
            const inputElement = this.$refs.input as HTMLInputElement;
            inputElement.value = city;
            this.search(city);
        },
        debounce(func: Function, timeout = 300) {
            let timer: number;
            return () => {
                clearTimeout(timer);
                timer = setTimeout(() => { func(); }, timeout);
            }
        },
        handleInputChange() {
            const inputElement = this.$refs.input as HTMLInputElement;
            const city = inputElement.value;
            this.search(city);
        },
        async search(city: string) {
            if (!city) {
                // Invalid string or user probably deleted string. No need to
                // search.
                return;
            }

            try {
                // Info about city received
                const res = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${import.meta.env.VITE_WEATHER_API}`);
                this.addValidCity(city);
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
        addValidCity(city: string) {
            if (this.recentCities.includes(city)) {
                // No need to add city to recent searches if it is already there
                // could have repeating values.
                return;
            }

            this.recentCities.push(city);
            if (this.recentCities.length > 5) {
                this.recentCities.shift();
            }
        },
    }
});
</script>