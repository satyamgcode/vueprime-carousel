<template>
  <div class="carousel-demo">
    <h2 class="text-center mb-4">PrimeVue Image Carousel</h2>
    <Carousel 
      :value="items" 
      :numVisible="5" 
      :numScroll="1" 
      :circular="true" 
      :responsiveOptions="responsiveOptions"
    >
      <template #item="slotProps">
        <div class="carousel-item">
          <img 
            :src="slotProps.data.image" 
            :alt="'Image ' + slotProps.index" 
            class="carousel-image"
          />
        </div>
      </template>
    </Carousel>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Carousel from 'primevue/carousel';
import image from './assets/images/check.jpg'
const items = ref([
  { image: image },
  { image: image },
  { image: image },
  { image: image },
  { image: image },
  { image: image },
  { image: image },
  { image: image },
  { image: image },
]);

const responsiveOptions = ref([
  {
    breakpoint: '1024px',
    numVisible: 3,
    numScroll: 1,
  },
  {
    breakpoint: '768px',
    numVisible: 2,
    numScroll: 1,
  },
  {
    breakpoint: '560px',
    numVisible: 1,
    numScroll: 1,
  },
]);
</script>

<style scoped>
.carousel-demo {
  padding: 20px;
  max-width: 100%;
  margin: 0 auto;
}

.carousel-item {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
}

.carousel-image {
  width: 100%;
  max-width: 250px;
  height: 60vh;
  object-fit: cover;
  border-radius: 8px;
}

.p-carousel-indicators {
  margin-top: 1rem;
}

</style>
