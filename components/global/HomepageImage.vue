<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

// Reactieve eigenschappen voor cirkelpositie
const circleStyle = ref({
  transform: 'translate(-50%, -50%)',
  top: '50%',
  left: '50%',
  transition: 'transform 0.1s ease-out',
});

let isReturningToCenter = false;
const maxDistance = 300; // Maximale afstand waarna de cirkel terugkeert naar het midden
const returnDuration = 500; // Duur van de terugkeer-animatie in milliseconden

function handleMouseMove(event) {
  if (isReturningToCenter) return;

  const centerX = window.innerWidth / 2;
  const centerY = window.innerHeight / 2;

  const dx = event.clientX - centerX;
  const dy = event.clientY - centerY;
  const distance = Math.sqrt(dx * dx + dy * dy);

  if (distance > maxDistance) {
    // Cirkel keert terug naar het midden
    isReturningToCenter = true;
    circleStyle.value = {
      transform: 'translate(-50%, -50%)',
      top: '50%',
      left: '50%',
      transition: `transform ${returnDuration}ms ease-in-out, filter 0.3s ease-in-out`,
       // Houd de blur consistent bij terugkeer
    };

    setTimeout(() => {
      isReturningToCenter = false;
    }, returnDuration);
  } else {
    // Cirkel volgt de muis zolang de afstand binnen de limiet is
    circleStyle.value = {
      transform: `translate(calc(-50% + ${dx / 2}px), calc(-50% + ${dy / 2}px))`,
      top: '50%',
      left: '50%',
      transition: 'transform 0.1s ease-out',
       // Zorg ervoor dat de blur consistent blijft
    };
  }
}

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove);
});

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove);
});
</script>

<template>
  <div>
    <div class="Blur" :style="circleStyle"></div>
    <div class="info flex flex-col items-center justify-center h-screen">
      <div class="TitelWidad text-4xl md:text-4xl lg:text-6xl">
        <p>PORTFOLIO</p>
      </div>
      <div class="SubtitleWidad">
        <div>
          <img class="ProfilePicture hidden md:block" src="/public/img/Profile_Picture.png" alt="" />
        </div>
        <div><p>Widad<br />Anncoud</p></div>
        <div><p>Motion Designer &<br />Graphic Designer</p></div>
        <div class="hidden md:block">
          <p>LUCA School of Arts MIND (2024-today)<br />Erasmushogeschool MCT (2020-2023)</p>
        </div>
        <div class="hidden md:block">
          <p>Adobe Softwares<br />TouchDesigner</p>
        </div>
        <div class="hidden md:block">
          <p>widad.acd@gmail.com<br />Bruxelles</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
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

.SubtitleWidad  { color: #000000; }
.dark-mode .SubtitleWidad { color: #fa5ec3f2;  }

.SubtitleWidad {
  top: 25%;
  width: 100%;
  margin-left: 7%;
  position: relative;
  z-index: 2;
  display: flex;
  font-family: "Space Mono", monospace;
  align-items: center;
}

.SubtitleWidad div {
  margin-right: 5%;
}

.ProfilePicture {
  width: 60pt;
}

.Blur {
  width: 400px;
  height: 400px;
  background-color: #fa5edb;
  border-radius: 50%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
  filter: blur(50px);
  transition: transform 0.1s ease-out, filter 0.3s ease-in-out;
}

.back {
  background-size: cover;
  background-position: center;
  height: 100%;
}
</style>
