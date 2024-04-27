<script setup>
const { meal, quantity, restaurant, selectedDishes } = defineProps(["meal", "quantity", "restaurant", "selectedDishes"]);
</script>

<template>
    <div class="row">
        <label>Meal:</label>
        <div>{{ meal }}</div>
    </div>
    <div class="row">
        <label>No. of people:</label>
        <div>{{ quantity }}</div>
    </div>
    <div class="row">
        <label>Restaurant:</label>
        <div>{{ restaurant }}</div>
    </div>
    <div class="row">
        <label>Dishes:</label>
        <div class="dishes">
            <div v-for="item of selectedDishes">{{ item.dishName }} - {{ item.quantity }}</div>
        </div>
    </div>
</template>

<style scoped>
.row {
    display: flex;
    gap: 10px;
    margin-bottom: 10px;
    label {
        width: 200px;
    }
}

.dishes{
    border: 1px solid #000;
    border-radius: 8px;
    padding: 10px;
}
</style>
