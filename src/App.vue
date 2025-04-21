<template>
  <div class="p-6 bg-[#E5F6F3] min-h-screen rounded-3xl">
    <h2 class="text-xl font-semibold mb-6">Image carousel</h2>

    <div class="tabs flex justify-center gap-4 mb-8">
      <button
        :class="['tab-button px-4 py-2 rounded-lg font-medium', activeTab === 'all' ? 'bg-blue-500 text-white' : 'bg-gray-200']"
        @click="setActiveTab('all')"
      >
        All Images
      </button>

      <button
        :class="['tab-button px-4 py-2 rounded-lg font-medium', activeTab === 'first10' ? 'bg-blue-500 text-white' : 'bg-gray-200']"
        @click="setActiveTab('first10')"
      >
        First 10 Images
      </button>

      <button
        :class="['tab-button px-4 py-2 rounded-lg font-medium', activeTab === 'next10' ? 'bg-blue-500 text-white' : 'bg-gray-200']"
        @click="setActiveTab('next10')"
      >
        Next 10 Images
      </button>
    </div>

    <!-- Carousel -->
    <div class="relative">
      <Carousel
        :value="filteredItems"
        :responsiveOptions="responsiveOptions"
        :numVisible="3"
        :numScroll="1"
        :circular="false"
        :showIndicators="false"
        style="width: 80vw; margin: 0 auto;"
        ref="carouselRef"
        :key="carouselKey"
        class="custom-carousel"
      >
        <template #item="slotProps">
          <div class="p-3">
            <div class="border rounded-xl shadow-md overflow-hidden bg-white">
              <img
                :src="slotProps.data.image"
                :alt="'Image ' + slotProps.index"
                class="w-full h-[200px] object-cover"
              />
              <div class="p-4">
                <h3 class="font-bold mb-2">title</h3>
                <p class="text-gray-700">This component is just for demo</p>
              </div>
            </div>
          </div>
        </template>
      </Carousel>

      <!-- Custom Pagination -->
      <div class="flex justify-center items-center gap-4 mt-8">
        <button @click="prev" class="text-orange-500 text-2xl">
          <i class="pi pi-angle-left"></i>
        </button>
        <div class="text-lg">
          <!-- <span class="font-bold">{{ currentPage }}</span> <span class="font-bold">{{ totalPages }}</span> -->
        </div>
        <button @click="next" class="text-orange-500 text-2xl">
          <i class="pi pi-angle-right"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue';
import Carousel from 'primevue/carousel';
import 'primeicons/primeicons.css';
import image from './assets/images/check.jpg';

const items = ref(
  Array.from({ length: 20 }, (_, i) => ({
    image: image,
    title: `Image ${i + 1}`
  }))
);

const activeTab = ref('first10');
const carouselRef = ref(null);
const carouselKey = ref(0);

const filteredItems = computed(() => {
  if (activeTab.value === 'first10') {
    return items.value.slice(0, 10);
  } else if (activeTab.value === 'next10') {
    return items.value.slice(10);
  } else {
    return items.value;
  }
});

const responsiveOptions = ref([
  {
    breakpoint: '1400px',
    numVisible: 3,
    numScroll: 1,
  },
  {
    breakpoint: '1199px',
    numVisible: 3,
    numScroll: 1,
  },
  {
    breakpoint: '991px',
    numVisible: 2,
    numScroll: 1,
  },
  {
    breakpoint: '767px',
    numVisible: 1,
    numScroll: 1,
  },
]);

const currentPage = ref(1);
const totalPages = computed(() => {
  return Math.ceil(filteredItems.value.length / 1); // because numScroll = 1
});

const next = () => {
  carouselRef.value?.navForward();
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
};

const prev = () => {
  carouselRef.value?.navBackward();
  if (currentPage.value > 1) {
    currentPage.value--;
  }
};

const setActiveTab = (tab) => {
  activeTab.value = tab;
  carouselKey.value += 1;
  currentPage.value = 1;
};

watch(activeTab, () => {
  currentPage.value = 1;
});

onMounted(() => {
  currentPage.value = 1;
});
</script>

<style scoped>
.custom-carousel .p-carousel-content {
  padding: 0;
}

.custom-carousel .p-carousel-container {
  margin-bottom: 2rem;
}
</style>
