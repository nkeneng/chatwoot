<script setup>
import { computed } from 'vue';

const props = defineProps({
  title: {
    type: String,
    default: '',
  },
  description: {
    type: String,
    default: '',
  },
  mediaUrl: {
    type: String,
    default: '',
  },
  price: {
    type: String,
    default: null,
  },
  actionLabel: {
    type: String,
    default: 'View',
  },
  url: {
    type: String,
    default: '#',
  },
});

const emit = defineEmits(['action-click']);

const hasContent = computed(() => {
  return props.title || props.description || props.mediaUrl;
});

const formattedPrice = computed(() => {
  if (typeof props.price === 'undefined' || props.price === null || props.price === '') {
    return null;
  }
  return props.price;
});

const handleActionClick = (e) => {
  e.preventDefault();
  emit('action-click', props);
};

const handleImageError = (e) => {
  e.target.style.display = 'none';
  const fallbackContainer = e.target.parentNode;
  
  // Check if fallback already exists
  if (!fallbackContainer.querySelector('.carousel-card__fallback-image')) {
    const fallbackSvg = document.createElement('div');
    fallbackSvg.className = 'carousel-card__fallback-image';
    fallbackSvg.innerHTML = `
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <rect x="3" y="3" width="18" height="18" rx="2" ry="2" />
        <circle cx="8.5" cy="8.5" r="1.5" />
        <polyline points="21,15 16,10 5,21" />
      </svg>
    `;
    fallbackContainer.appendChild(fallbackSvg);
  }
};
</script>

<template>
  <div v-if="hasContent" class="carousel-card">
    <div class="carousel-card__image">
      <img
        v-if="mediaUrl"
        :src="mediaUrl"
        :alt="title || 'Card image'"
        @error="handleImageError"
      />
      <div v-else class="carousel-card__fallback-image">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <rect x="3" y="3" width="18" height="18" rx="2" ry="2" />
          <circle cx="8.5" cy="8.5" r="1.5" />
          <polyline points="21,15 16,10 5,21" />
        </svg>
      </div>
    </div>
    <div class="carousel-card__info">
      <div class="carousel-card__name">
        {{ title || 'Unnamed Product' }}
      </div>
      <div v-if="formattedPrice || actionLabel" class="carousel-card__price-action">
        <div v-if="formattedPrice" class="carousel-card__price">
          {{ formattedPrice }}
        </div>
        <a
          class="carousel-card__action"
          :href="url"
          @click="handleActionClick"
        >
          {{ actionLabel }}
          <svg class="carousel-card__action-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M9 5l7 7-7 7" />
          </svg>
        </a>
      </div>
    </div>
  </div>
</template>

<style scoped>
.carousel-card {
  background-color: var(--color-background, #ffffff);
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  transition: all 0.2s ease;
  width: 100%;
  max-width: 300px; /* Bumped up for taller view */
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  height: auto;
  box-sizing: border-box;
}

.carousel-card:hover {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  transform: translateY(-2px);
}

.carousel-card__image {
  width: 100%;
  height: 220px; /* Increased for taller image display */
  background-color: transparent;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.carousel-card__image img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.carousel-card__fallback-image {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  color: var(--color-text-secondary, #6b7280);
  background-color: var(--color-background-light, #f9fafb);
}

.carousel-card__fallback-image svg {
  width: 48px;
  height: 48px;
}

.carousel-card__info {
  padding: 1rem;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.carousel-card__name {
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
  line-height: 1.2;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  line-clamp: 2;
  overflow: hidden;
  color: var(--color-text-primary, #111827);
}

.carousel-card__price-action {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 8px;
}

.carousel-card__price {
  font-weight: 600;
  font-size: 0.85rem;
  color: var(--color-text-primary, #111827);
}

.carousel-card__action {
  color: var(--color-primary, #1d4ed8);
  text-decoration: none;
  font-size: 0.75rem;
  display: flex;
  align-items: center;
  transition: color 0.2s ease;
}

.carousel-card__action:hover {
  color: var(--color-primary-dark, #1e40af);
}

.carousel-card__action-icon {
  width: 12px;
  height: 12px;
  margin-left: 0.25rem;
  fill: none;
  stroke: currentColor;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .carousel-card {
    max-width: 280px; /* Larger on tablets for more height */
  }
  
  .carousel-card__image {
    height: 200px; /* Tablet image height bump */
  }
}

@media (max-width: 480px) {
  .carousel-card {
    max-width: 260px; /* Mobile width bump for height */
  }
  
  .carousel-card__image {
    height: 180px; /* Mobile image height bump */
  }
  
  .carousel-card__info {
    padding: 0.75rem;
  }
}
</style>
