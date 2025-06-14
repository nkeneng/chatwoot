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

<style scoped>

/* Carousel Styles based on React ProductCard implementation */
.chat-card-carousel {
  width: 100%;
  max-width: 100%; /* Ensure it doesn't overflow */
  padding: 1rem 0;
  background: transparent;
  overflow: visible; /* Allow peek of adjacent slides */
  box-sizing: border-box;
}

/* Splide container adjustments */
.chat-card-carousel :deep(.splide) {
  padding: 0;
  background: transparent;
}

.chat-card-carousel :deep(.splide__track) {
  overflow: visible;
  background: transparent;
}

.chat-card-carousel :deep(.splide__list) {
  overflow: visible;
  background: transparent;
}

.chat-card-carousel :deep(.splide__slide) {
  opacity: 0.6;
  transition: all 0.3s ease;
  transform: scale(0.95);
}

.chat-card-carousel :deep(.splide__slide.is-active),
.chat-card-carousel :deep(.splide__slide.is-visible) {
  opacity: 1;
  transform: scale(1);
}

/* Override CarouselCard styles for this specific carousel implementation */
.chat-card-carousel :deep(.splide__slide .carousel-card) {
  width: 100%;
  margin: 0;
}

/* Splide arrow customizations */
.chat-card-carousel :deep(.splide__arrow) {
  background: var(--color-background, #ffffff) !important;
  border: 1px solid var(--color-border, #e5e7eb) !important;
  opacity: 0.9;
  transition: all 0.3s ease;
  width: 32px !important;
  height: 32px !important;
  border-radius: 50% !important;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06) !important;
}

.chat-card-carousel :deep(.splide__arrow:hover) {
  opacity: 1;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06) !important;
}

.chat-card-carousel :deep(.splide__arrow:disabled) {
  opacity: 0.4;
}

.chat-card-carousel :deep(.splide__arrow svg) {
  fill: var(--color-text-primary, #374151) !important;
  width: 16px !important;
  height: 16px !important;
}

.chat-card-carousel :deep(.splide__arrow--prev) {
  left: -12px; /* Reduced from -16px */
  z-index: 2;
}

.chat-card-carousel :deep(.splide__arrow--next) {
  right: -12px; /* Reduced from -16px */
  z-index: 2;
}

/* Ensure arrows are visible on mobile */
@media (max-width: 480px) {
  .chat-card-carousel :deep(.splide__arrow--prev) {
    left: -6px; /* Reduced from -8px */
  }
  
  .chat-card-carousel :deep(.splide__arrow--next) {
    right: -6px; /* Reduced from -8px */
  }
}
</style>
