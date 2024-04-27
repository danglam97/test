<script setup>
import { ref } from "vue";

const { onChange, meal, quantity } = defineProps(["onChange", "meal", "quantity"]);

const options = ref([
    {
        value: "breakfast",
        label: "Breakfast",
    },
    {
        value: "lunch",
        label: "Lunch",
    },
    {
        value: "dinner",
        label: "Dinner",
    },
]);
</script>

<template>
    <div>
        <label>Please select a meal: </label><br />
        <a-select
            :value="meal"
            style="width: 200px"
            :options="options"
            @change="(value) => onChange('meal', value)"
            placeholder="Choose a meal"
        ></a-select>
    </div>
    <div>
        <label>Please enter Number of People: </label><br />
        <a-input-number
            placeholder="Input a number"
            style="width: 200px"
            :value="quantity"
            :min="1"
            :max="10"
            @change="(value) => onChange('quantity', value)"
        />
    </div>
</template>
