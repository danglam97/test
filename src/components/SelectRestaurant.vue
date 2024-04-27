<script setup>

const { onChange, restaurant, restaurantOptions } = defineProps(["onChange", "restaurant", "restaurantOptions"]);

</script>

<template>
    <div>
        <label>Please select a restaurant: </label><br />
        <a-select
            :value="restaurant"
            style="width: 200px"
            :options="restaurantOptions"
            @change="onChange"
            placeholder="Choose a restaurant"
        ></a-select>
    </div>
</template>
