<template >
    <div class="weather-info-item">
        <span>{{ name }}:</span> <span>{{ value }}</span>
    </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    props: {
        name: String,
        value: String
    }
});
</script>
<style scoped>
    .weather-info-item {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        column-gap: 30px;
    }
</style>