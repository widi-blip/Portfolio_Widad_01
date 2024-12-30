<script setup>
import { ref, onMounted } from 'vue';

// Reactieve eigenschappen voor cirkelpositie
const circleStyle = ref({
  transform: 'translate(0, 0)', // Default transform
});

// Muisbeweging-afhandelingsfunctie
function handleMouseMove(event) {
  const circle = document.querySelector('.Blur'); // Selecteer de cirkel
  if (circle) {
    const rect = circle.getBoundingClientRect();
    const centerX = rect.left + rect.width / 2; // Bepaal middelpunt X
    const centerY = rect.top + rect.height / 2; // Bepaal middelpunt Y

    const dx = (event.clientX - centerX) / 20; // Beweging horizontaal
    const dy = (event.clientY - centerY) / 20; // Beweging verticaal

    circleStyle.value = {
      transform: `translate(${dx}px, ${dy}px)`,
      transition: 'transform 0.1s ease-out', // Zachte overgang
    };
  }
}

// Eventlistener toevoegen en verwijderen
onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove); // Voeg luisteraar toe
});
onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove); // Verwijder luisteraar
});

const settings = ref(null);  // Initialize as null for loading state
const isLoading = ref(true);  // Loading state to show the content only after data is loaded
const hasError = ref(false);  // Error state to show an error message if data loading fails
// Fetch JSON data using native fetch API
onMounted(async () => {
  try {
    const response = await fetch('../_data/homepage.json');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const jsonData = await response.json();
    settings.value = jsonData;
    isLoading.value = false;  // Stop loading when data is fetched
  } catch (error) {
    hasError.value = true;  // If there's an error, show the error message
    console.error('Error loading settings:', error);
  }
});
</script>

<template>

    <div>
    <div v-if="isLoading" class="flex items-center justify-center h-screen">
      <p>Loading...</p> <!-- Display loading text or spinner while data is being fetched -->
    </div>
    <div v-else-if="hasError" class="flex items-center justify-center h-screen">
      <p>Error loading homepage settings. Please try again later.</p> <!-- Display error message -->
    </div>
    
    <div v-else
      class="back bg-cover bg-center h-full"
      :style="{ backgroundImage: settings.thumbnail ? `url(${settings.thumbnail})` : '' }" 
    >

      <!-- Info section that triggers the hover effect -->
      <div class="info flex flex-col items-center justify-center h-screen">
        <div class="TitelWidad text-4xl md:text-4xl lg:text-6xl"><p>PORTFOLIO</p></div>
        <div class="SubtitleWidad">
          <div><img class="ProfilePicture hidden md:block" src="/public/img/Profile_Picture.png" alt=""></div>
          <div class=""> <p>Widad<br>Anncoud</p></div>
          <div class=""><p>Motion Designer &<br>Graphic Designer</p> </div>
          <div class="hidden md:block"><p>LUCA School of Arts MIND (2024-today)<br>Erasmushogeschool MCT (2020-2023)</p> </div>
          <div class="hidden md:block"> <p>Adobe Softwares<br>TouchDesigner</p></div>
          <div class="hidden md:block"> <p>widad.acd@gmail.com<br>Bruxelles</p></div>
          
        </div>
        <div class="Blur" :style="circleStyle"></div>
      </div>
  </div>
</div>  
</template>

<style scoped>
/* Define the background color for the page */
.TitelWidad {
  top: -2%;
  
  position: relative;
  z-index: 2;
  color: rgb(0, 0, 0);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: "Space Mono", monospace;
}

.SubtitleWidad {
  top: 25%;
  width: 100%;
  margin-left: 7%;
  position: relative;
  z-index: 2;
  color: rgb(0, 0, 0);
  display: flex;
  font-family: "Space Mono", monospace;
  
  align-items: center; /* aligns the items vertically */
}

.SubtitleWidad div {
  margin-right: 5%;
}

.ProfilePicture{
  width: 60pt;
}

.space-mono-regular {
  font-family: "Space Mono", monospace;
  font-weight: 700;
  font-style: normal;
}

.Blur {
  top: 20%;
  bottom: 20%;
  background-color: #fa5edb;
  padding: 15%;
  border-radius: 50%;
  filter: blur(50px);
  position: absolute;
  z-index: 1;
}
.pr-5{
  position: sticky;
  top: 0;
}
</style> 