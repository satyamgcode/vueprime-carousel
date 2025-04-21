<template>
  <div class="p-6 bg-[#E5F6F3] min-h-screen rounded-3xl">
    <h2 class="text-xl font-semibold mb-6">Image carousel</h2>
    <div class="mb-4 text-center">
      <p>click to see demo code</p>
      <a href="../src/assets/demo_code.docx" class="text-blue-500 underline">Demo code</a>
    </div>

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
        ref="carouselRef"
        :key="carouselKey"
        class="max-w-[80vw] mx-auto"
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
                <h3 class="font-bold mb-2">{{ slotProps.data.title }}</h3>
                <p class="text-gray-700">This component is just for demo</p>
              </div>
            </div>
          </div>
        </template>
      </Carousel>

      <div class="flex justify-center items-center gap-4 mt-8">
        <button @click="prev" class="text-orange-500 text-2xl">
          <i class="pi pi-angle-left"></i>
        </button>
        <button @click="next" class="text-orange-500 text-2xl">
          <i class="pi pi-angle-right"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch, onMounted } from 'vue';
import Carousel from 'primevue/carousel';
import 'primeicons/primeicons.css';
import image from './assets/images/check.jpg';

interface Item {
  image: string;
  title: string;
}

interface ResponsiveOption {
  breakpoint: string;
  numVisible: number;
  numScroll: number;
}

const items = ref<Item[]>(
  Array.from({ length: 20 }, (_, i) => ({
    image: image,
    title: `Image ${i + 1}`,
  }))
);

type Tab = 'all' | 'first10' | 'next10';
const activeTab = ref<Tab>('first10');
const carouselRef = ref<InstanceType<typeof Carousel> | null>(null);
const carouselKey = ref<number>(0);

const filteredItems = computed<Item[]>(() => {
  if (activeTab.value === 'first10') {
    return items.value.slice(0, 10);
  } else if (activeTab.value === 'next10') {
    return items.value.slice(10);
  } else {
    return items.value;
  }
});

const responsiveOptions = ref<ResponsiveOption[]>([
  { breakpoint: '1400px', numVisible: 3, numScroll: 1 },
  { breakpoint: '1199px', numVisible: 3, numScroll: 1 },
  { breakpoint: '991px', numVisible: 2, numScroll: 1 },
  { breakpoint: '767px', numVisible: 1, numScroll: 1 },
]);

const currentPage = ref<number>(1);
const totalPages = computed<number>(() => {
  return Math.ceil(filteredItems.value.length / 1);
});

const next = (): void => {
  carouselRef.value?.navForward();
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
};

const prev = (): void => {
  carouselRef.value?.navBackward();
  if (currentPage.value > 1) {
    currentPage.value--;
  }
};

const setActiveTab = (tab: Tab): void => {
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
  max-width: 80vw !important;
}

.custom-carousel .p-carousel-container {
  margin-bottom: 2rem;
}
</style>
