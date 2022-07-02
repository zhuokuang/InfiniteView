<script setup>
import { ref } from 'vue';
import InfiniteView from './components/InfiniteView.vue';

const nextPage = ref(1);
const list = ref(new Array(40).fill(0));

const onload = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      list.value.push(...new Array(50).fill(0).map((_, i) => i + 1 + (nextPage.value - 1) * 50));
      nextPage.value++;
      resolve(nextPage.value);
    }, 200);
  });
}
</script>

<template>
  <InfiniteView :onload="onload">
    <div v-for="item in list">
      {{ item }}
    </div>
  </InfiniteView>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  height: 100vh;
}
</style>
