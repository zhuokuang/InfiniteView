<script setup>
import { ref, defineProps, onMounted } from 'vue';

const props = defineProps({
  distance: {
    type: Number,
    default: 30,
  },
  onload: {
    type: Function,
    default: async () => {},
  },
  classStyle: {
    type: Object,
    default: () => ({}),
  },
});

const isloading = ref(false);

const onScroll = async (element) => {
  if (isloading.value) {
    return;
  }
  if (element.scrollHeight <= element.scrollTop + element.clientHeight + props.distance) {
    try {
      isloading.value = true;
      await props.onload();
      isloading.value = false;
    } catch (error) {
      console.log(error);
      isloading.value = false;
    }
  }
}

onMounted(() => {
  window.addEventListener('scroll', (e) => {
    onScroll(e.target);
  })
})
</script>

<template>
  <div
    class="infinite-view"
    :style="classStyle"
    @scroll="onScroll($event.target)"
  >
    <slot />
  </div>
</template>

<style scoped>
.infinite-view {
  height: 100%;
  overflow-y: scroll;
}
</style>
