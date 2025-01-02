<template>
  <div class="layout2">
    <ClientOnly>
      <div class="relative max-h-screen overflow-hidden z-1">
        <div
          class="absolute inset-0 flex flex-col items-center justify-center bg-black bg-opacity-50 animate-fade animate-once animate-delay-[500ms]"
        >
          <div class="container p-4">
            <div>
              <h1 class="PageTitle text-white text-4xl md:text-6xl lg:text-8xl font-bold">{{ data.title }}</h1>
              <h1 v-if="data.subtitle" class="text-white opacity-80 pt-3 text-xl md:text-2xl lg:text-3xl font-bold pb-10">{{ data.subtitle }}</h1>
            </div>

            <div>
              <p v-if="data.author" class="text-white opacity-80 text-xs font-bold">{{ data.author }}</p>
              <p class="text-white text-xs opacity-50 hover:opacity-100">Last update: {{ formatDate(data.date) }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Lazy loading Drawer component -->
      <div class="absolute top-0 right-0 z-10 pr-5">
        <Drawer v-lazy /> <!-- Lazy directive or conditional rendering -->
      </div>

      <!-- Main section -->
      <div class="container mx-auto p-4 animate-fade animate-once animate-delay-[500ms]">
        <h1 class="PageTitle2 md:text-3xl lg:text-6xl lg:mb-3 font-bold">{{ data.title }}</h1>
        <div class="masonry-gallery mt-6">
          <!-- Image gallery -->
          <div v-if="data.imagegallery && data.imagegallery.showgallery == true">
            <div class="masonry-grid">
              <div v-for="(image, index) in data.imagegallery.galleryImages" :key="index" class="masonry-item">
                <img :src="image" :alt="'Image ' + (index + 1)" class="masonry-image" />
              </div>
            </div>
          </div>
        </div>

        <div class="SecondColumn">
          <ContentRenderer :value="data" v-lazy /> <!-- Lazy load ContentRenderer -->
        </div>

        <!-- Link and published date -->
        <div class="text-xs leading-3">
          <hr />
          <p class="text-xs opacity-50 hover:opacity-100 pb-2">Last update: {{ formatDate(data.date) }}</p>
          <article v-if="data.tags" class="tags">
            <li v-for="(item, index) in data.tags" :key="index" class="pt-2 text-xs opacity-50 hover:opacity-100">
              <NuxtLink :to="`/tags/${item}`" v-lazy>{{ item }}</NuxtLink> <!-- Lazy load NuxtLink -->
            </li>
          </article>
        </div>
      </div>

      <!-- Lazy loading the ShareButtons component -->
      <ShareButtons v-lazy />

      <!-- SEO metadata -->
      <Title>{{ data.title }}</Title>
      <Meta name="description" :content="data.description" />
      <Meta name="tags" :content="data.tags.join(', ') || ''" />
      <Meta name="keywords" :content="data.tags.join(', ') || ''" />
      <Meta property="og:title" :content="data.title" />
      <Meta property="og:description" :content="data.description" />
      <Meta property="og:url" :content="data.url" />
      <Meta property="og:type" content="article" />
    </ClientOnly>
  </div>
</template>

<script setup>
import { ref } from 'vue';
const imageLoaded = ref(true); // Thumbnail removed, always loaded

defineProps(['data', 'formatDate']);
</script>

<style scoped>
.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #fff;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
}

.PageTitle {
  font-family: "Space Mono", monospace;
}

.PageTitle2 {
  font-family: "Space Mono", monospace;
  color: #000000;
  font-size: 50pt;
  padding: 30pt 0 10pt 0;
}

.SecondColumn {
  margin-top: 20pt;
}

.masonry-gallery {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 20px;
}

.masonry-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(450px, 1fr)); /* Increased min size for larger images */
  grid-gap: 25px;
}

.masonry-item {
  position: relative;
  width: 100%;
  height: auto;
  margin-bottom: 25px;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.masonry-image {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Ensures images scale proportionally */
}

.masonry-item:hover {
  transform: scale(1.1); /* Slight zoom effect on hover */
}

.masonry-item:nth-child(odd) {
  height: 500px; /* Increased height for odd items */
}

.masonry-item:nth-child(even) {
  height: 450px; /* Increased height for even items */
}

@media (max-width: 768px) {
  .masonry-grid {
    grid-template-columns: repeat(auto-fill, minmax(100%, 1fr)); /* Full width for smaller screens */
  }
  .masonry-item {
    height: 300px; /* Adjusted height for smaller screens */
  }
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
