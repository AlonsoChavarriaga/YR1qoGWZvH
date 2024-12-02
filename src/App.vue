<script setup lang="ts">
import { ref, computed, watchEffect } from 'vue';
import SliderControl from './components/SliderControl.vue';
import Swatch from './components/Swatch.vue';
import type { ColorSwatchData } from './components/Swatch.vue';

const TOTAL_HUES = 360;

const saturation = ref<number>(25);
const lightness = ref<number>(50);
const colorsToDisplay = ref<ColorSwatchData[]>([]);

const generatedFullHSLColorRange = computed(() => {
  return Array.from({ length: TOTAL_HUES }, (_, hue) => ({
    hsl: `hsl(${hue}, ${saturation.value}%, ${lightness.value}%)`,
    hue,
  }));
});

const fetchColorData = () => {
  const colors = generatedFullHSLColorRange.value;
  const colorList = colors.map((color) => {
    const hslString = `hsl(${color.hue},${saturation.value}%,${lightness.value}%)`;

    return {
      hsl: hslString,
      hue: color.hue,
      name: `Color ${color.hue}`,
      rgb: '#000000'
    };
  });

  colorsToDisplay.value = colorList;
};

watchEffect(() => {
  fetchColorData();
});
</script>

<template>
  <main id="app">
    <h1>Color Swatch</h1>

    <div class="controls">
      <SliderControl label="Saturation" v-model="saturation" />
      <SliderControl label="Lightness" v-model="lightness" />
    </div>

    <div class="swatch-grid">
      <Swatch v-for="color in colorsToDisplay" :key="color.hue" :color="color" />
    </div>
  </main>
</template>

<style scoped>
#app {
  margin: 0 auto;
  padding: 0 var(--section-gap);
}

.controls {
  margin: 2rem 0;
}

.swatch-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}


header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
