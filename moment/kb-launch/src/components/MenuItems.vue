<template>
  <div>{{ props.foodName }}</div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  foodName: {
    Type: String,
  },
  idx: {
    Type: Number,
  },
});

console.log(props);

// const propName = ref(props.foodName);
</script>

<style scoped>
div {
  color: #a0a0a0;
  border: 2px solid #dcdcdc;
  border-radius: 15px;
  padding: 5px 10px;
}
</style>
