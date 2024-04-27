<script setup>

const { onChange, dishes, dishOptions, handleAdd } = defineProps(["onChange", "dishes", "dishOptions", "handleAdd"]);

</script>

<template>
    <div class="dish-row" v-for="(item, index) of dishes">
        <div>
            <label>Please select a dish: </label><br />
            <a-select
                :value="item.dish"
                style="width: 200px"
                :options="dishOptions"
                @change="(value) => onChange('dish', index, value)"
                placeholder="Choose a dish"
            ></a-select>
        </div>
        <div>
            <label>Please enter number of serving: </label><br />
            <a-input-number
                placeholder="Input a number"
                style="width: 200px"
                :value="item.quantity"
                :min="1"
                :max="10"
                @change="(value) => onChange('quantity', index, value)"
            />
        </div>
    </div>
    <a-button type="primary" :disabled="dishes.length >= 5" @click="handleAdd">Add</a-button>
</template>

<style scoped>
.dish-row{
    display: flex;
    gap: 40px;
    align-items: center;
    margin-bottom: 10px;
}

</style>
