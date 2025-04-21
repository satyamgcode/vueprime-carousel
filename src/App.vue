<template>
  <div class="mx-auto">
    <div class="tabs flex justify-center gap-4 mb-4 mt-4" >
            <button
        :class="['tab-button px-4 py-2 rounded-lg', activeTab === 'all' ? 'bg-blue-500 text-white' : 'bg-gray-200']"
        @click="setActiveTab('all')"
      >
        All Images
      </button>

      <button
        :class="['tab-button px-4 py-2 rounded-lg', activeTab === 'first10' ? 'bg-blue-500 text-white' : 'bg-gray-200']"
        @click="setActiveTab('first10')"
      >
        First 10 Image
      </button>
      <button
        :class="['tab-button px-4 py-2 rounded-lg', activeTab === 'next10' ? 'bg-blue-500 text-white' : 'bg-gray-200']"
        @click="setActiveTab('next10')"
      >
        Next 10 Images
      </button>
    </div>

    <!-- Carousel -->
    <Carousel
      :value="filteredItems"
      :responsiveOptions="responsiveOptions"
      :numVisible="5"
      :numScroll="1"
      :circular="false"
      style="max-width:80vw; width: 80vw; margin: 0 auto;"
      :show-indicators="false"
      ref="carouselRef"
      :key="carouselKey"
    >
      <template #item="slotProps">
        <div class="p-4">
          <div
            class="carousel-slide border border-surface-200 dark:border-surface-700 rounded-xl p-4 cursor-pointer"
          >
            <img
              :src="slotProps.data.image"
              :alt="'Image ' + slotProps.index"
              class="carousel-image w-full h-auto object-cover rounded-lg max-h-[300px]"
              @click="selectCover(slotProps.data.title)"
            />
            <p>
              {{ slotProps.data.title }}
            </p>
          </div>
        </div>
      </template>
    </Carousel>
  </div>
</template>

<script setup>
import { ref, computed, nextTick } from 'vue';
import Carousel from 'primevue/carousel';
import image from './assets/images/check.jpg';

const items = ref([
  { image: image, title: 'Image 1' },
  { image: image, title: 'Image 2' },
  { image: image, title: 'Image 3' },
  { image: image, title: 'Image 4' },
  { image: image, title: 'Image 5' },
  { image: image, title: 'Image 6' },
  { image: image, title: 'Image 7' },
  { image: image, title: 'Image 8' },
  { image: image, title: 'Image 9' },
  { image: image, title: 'Image 10' },
  { image: image, title: 'Image 11' },
  { image: image, title: 'Image 12' },
  { image: image, title: 'Image 13' },
  { image: image, title: 'Image 14' },
  { image: image, title: 'Image 15' },
  { image: image, title: 'Image 16' },
  { image: image, title: 'Image 17' },
  { image: image, title: 'Image 18' },
  { image: image, title: 'Image 19' },
  { image: image, title: 'Image 20' },
]);

const responsiveOptions = ref([
  {
    breakpoint: '1400px',
    numVisible: 5,
    numScroll: 1,
  },
  {
    breakpoint: '1199px',
    numVisible: 4,
    numScroll: 1,
  },
  {
    breakpoint: '991px',
    numVisible: 3,
    numScroll: 1,
  },
  {
    breakpoint: '767px',
    numVisible: 2,
    numScroll: 1,
  },
  {
    breakpoint: '575px',
    numVisible: 1,
    numScroll: 1,
  },
]);

const activeTab = ref('first10');
const carouselRef = ref(null);

// Dynamic key for carousel
const carouselKey = ref(0);

// Computed property for filtered items
const filteredItems = computed(() => {
  if (activeTab.value === 'first10') {
    return items.value.slice(0, 10);
  } else if (activeTab.value === 'next10') {
    return items.value.slice(10);
  } else if (activeTab.value === 'all') {
    return items.value; // Return all items
  }
  return [];
});


// Function to switch tabs and reset the carousel
const setActiveTab = (tab) => {
  activeTab.value = tab;

  // Increment carouselKey to force a re-render
  carouselKey.value += 1;
};

// Function to handle image click (optional functionality for the example)
const selectCover = (title) => {
  console.log(`Image titled "${title}" selected`);
};
</script>
