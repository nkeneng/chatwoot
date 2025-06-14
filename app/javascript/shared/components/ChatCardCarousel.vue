<script setup>
import { Splide, SplideSlide } from '@splidejs/vue-splide';
import { toRefs, computed } from 'vue';
import '@splidejs/vue-splide/css';
import CarouselCard from './CarouselCard.vue';

const props = defineProps({
  items: { type: Array, default: () => [] },
});
const { items } = toRefs(props);

// Configure Splide options similar to your React implementation
const splideOptions = computed(() => {
  const windowWidth = typeof window !== 'undefined' ? window.innerWidth : 1024;
  const isSmallMobile = windowWidth <= 480;
  // Optimized padding for larger cards to show good preview effect
  const padding = isSmallMobile ? '2rem' : '1.5rem';

  return {
    perPage: 1,
    pagination: false,
    arrows: true,
    focus: 'center',
    padding: padding,
    gap: '0.75rem', // Balanced gap
    breakpoints: {
      768: {
        padding: '1.5rem',
        arrows: true
      },
      480: {
        padding: '2rem',
        arrows: true
      }
    }
  };
});

// Filter out any invalid items
const validItems = computed(() => {
  return items.value.filter(item => item && (item.title || item.media_url || item.mediaUrl));
});

// Handle carousel item actions
const handleItemAction = (item) => {
  console.log('Item action clicked:', item);
  // Add your action handling logic here
};
</script>

<template>
  <div v-if="validItems.length > 0" class="chat-card-carousel">
    <Splide :options="splideOptions" class="w-full">
      <SplideSlide v-for="(item, index) in validItems" :key="`${item.title}-${index}`">
        <CarouselCard
          :media-url="item.media_url || item.mediaUrl"
          :title="item.title"
          :description="item.description"
          :price="item.price"
          :action-label="item.actionLabel || 'View'"
          :url="item.url || '#'"
          @action-click="handleItemAction"
        />
      </SplideSlide>
    </Splide>
  </div>
</template>
