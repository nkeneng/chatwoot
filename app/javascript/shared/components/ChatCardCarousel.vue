<script setup>
import { Splide, SplideSlide } from '@splidejs/vue-splide';
import { toRefs } from 'vue';
import '@splidejs/vue-splide/css';
import ChatCard from './ChatCard.vue';

const props = defineProps({
  items: { type: Array, default: () => [] },
});
const { items } = toRefs(props);
</script>

<template>
  <Splide :options="{ pagination: false, arrows: true }" class="w-full">
    <SplideSlide v-for="item in items" :key="item.title">
      <ChatCard
        :media-url="item.media_url"
        :title="item.title"
        :description="item.description"
        :actions="item.actions"
        class="mx-1"
      />
    </SplideSlide>
  </Splide>
</template>
