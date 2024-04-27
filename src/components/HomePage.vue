<script setup>
import { ref, watchEffect } from "vue";
import selectMeal from "./SelectMeal.vue";
import selectRestaurant from "./SelectRestaurant.vue";
import selectDish from "./SelectDish.vue";
import preview from "./Preview.vue"
import dishes from "../../data/dishes.json";

const activeKey = ref("1");
const meal = ref();
const quantity = ref();
const restaurant = ref();
const restaurantOptions = ref(
    dishes.dishes.map((item) => ({
        label: item.restaurant,
        value: item.restaurant,
        availableMeals: item.availableMeals,
    }))
);
const selectedDishes = ref([{ dish: undefined, dishName: undefined, quantity: 1 }]);
const dishOptions = ref([]);
const disabled = ref({ prev: true, next: true });

const handleClick = (type) => {
    activeKey.value =
        type === "prev"
            ? (Number(activeKey.value) - 1).toString()
            : type === "next"
            ? (activeKey.value = (Number(activeKey.value) + 1).toString())
            : "1";
};

const onMealChange = (type, value) => {
    console.log(type)

    switch (type) {
        case "meal":
            meal.value = value;
            break;

        case "quantity":
            quantity.value = value;
            break;

        default:
            break;
    }
    disabled.value = { prev: activeKey.value <= 1, next: !(meal.value && quantity.value) };
    restaurantOptions.value = dishes.dishes
      .filter((item, index) => item.availableMeals.includes(meal.value) && dishes.dishes.findIndex(subItem => subItem.restaurant === item.restaurant) === index)
      .map((item) => ({
          label: item.restaurant,
          value: item.restaurant,
          availableMeals: item.availableMeals,
          name: item.name,
      }));
};

const onRestaurantChange = (value) => {
    restaurant.value = value;
    disabled.value = { prev: activeKey.value <= 1, next: !value };
    dishOptions.value = dishes.dishes
        .filter((item, index) => item.restaurant === restaurant.value && dishes.dishes.findIndex(subItem => subItem.name === item.name) === index)
        .map((item) => ({ label: item.name, value: item.id }));
};

const onDishChange = (type, index, value) => {
    const newValue = selectedDishes.value[index];
    newValue[type] = value;
    if(type === 'dish'){
      newValue.dishName = dishes.dishes.find(item => item.id === value)?.name
    }
    
};

const handleAddDish = () => {
    selectedDishes.value = selectedDishes.value.concat({ dish: undefined, quantity: 1 });
};

const handleSubmit = ()=>{
  console.log('submit')
}

watchEffect(() => {
  const invalidDishes = selectedDishes.value.length === 0 || selectedDishes.value.find(item => !item.dish || item.quantity < 1) 
    disabled.value =
        activeKey.value === "1"
            ? { prev: activeKey.value <= 1, next: !(meal.value && quantity.value) }
            : activeKey.value === "2"
              ? { prev: activeKey.value <= 1, next: !restaurant.value }
              : activeKey.value === "3"
                ? { prev: activeKey.value <= 1, next: invalidDishes }
                : activeKey.value === "4"
                  ? { prev: activeKey.value <= 1, next: true}
                  : { prev: false, next: false }
});
</script>

<template>
    <div class="tab-container">
        <a-tabs v-model:activeKey="activeKey">
            <a-tab-pane disabled key="1" tab="Step 1">
                <selectMeal :onChange="onMealChange" :meal="meal" :quantity="quantity" />
            </a-tab-pane>
            <a-tab-pane disabled key="2" tab="Step 2">
                <selectRestaurant
                    :onChange="onRestaurantChange"
                    :restaurant="restaurant"
                    :restaurantOptions="restaurantOptions"
                />
            </a-tab-pane>
            <a-tab-pane disabled key="3" tab="Step 3">
                <selectDish
                    :onChange="onDishChange"
                    :dishes="selectedDishes"
                    :dishOptions="dishOptions"
                    :handleAdd="handleAddDish"
                />
            </a-tab-pane>
            <a-tab-pane disabled key="4" tab="Preview">
                <preview :meal="meal" :quantity="quantity" :restaurant="restaurant" :selectedDishes="selectedDishes"/>
            </a-tab-pane>
        </a-tabs>
    </div>
    <div class="button-container">
        <a-button type="primary" :disabled="disabled.prev" @click="handleClick('prev')">Previous</a-button>
        <a-button type="primary" v-if="activeKey < 4" :disabled="disabled.next" @click="handleClick('next')">Next</a-button>
        <a-button type="primary" v-else @click="handleSubmit">Submit</a-button>
    </div>
</template>

<style scoped lang="css">
.tab-container {
    margin-bottom: 20px;
    height: 450px;
}
.button-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
</style>
