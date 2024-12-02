<script setup lang="ts">
import { ref, computed, watch } from 'vue';
import { debounce } from 'moderndash';
import SliderControl from './components/SliderControl.vue';
import Swatch from './components/Swatch.vue';
import type { ColorSwatchData } from './components/Swatch.vue';

const TOTAL_HUES = 360;

const saturation = ref<number>(25);
const lightness = ref<number>(50);
const colorsToDisplay = ref<ColorSwatchData[]>([]);
const isLoading = ref<boolean>(false);

const generatedFullHSLColorRange = computed(() => {
  return Array.from({ length: TOTAL_HUES }, (_, hue) => ({
    hsl: `hsl(${hue}, ${saturation.value}%, ${lightness.value}%)`,
    hue,
  }));
});

const fetchColorData = async () => {
  isLoading.value = true;

  try {
    const colors = generatedFullHSLColorRange.value;
    const colorDataPromises = colors.map(async (color): Promise<ColorSwatchData> => {
      const hslString = `${color.hue},${saturation.value}%,${lightness.value}%`;
      const response = await fetch(
        `https://www.thecolorapi.com/id?hsl=${encodeURIComponent(hslString)}`
      );

      if (!response.ok) {
        throw new Error(`Error: ${response.status}`);
      }

      const data = await response.json();

      return {
        ...color,
        name: data.name.value as string,
        rgb: data.rgb.value as string,
      };
    });

    const results = await Promise.all(colorDataPromises);
    colorsToDisplay.value = results;
  } catch (error) {
    console.error('Error:', error);
  } finally {
    isLoading.value = false;
  }
}

const debouncedFetchColorData = debounce(fetchColorData, 300);

watch([saturation, lightness], () => {
  debouncedFetchColorData();
}, { immediate: true });
</script>

<template>
  <main id="app">
    <h1>Color Swatch</h1>

    <div class="controls">
      <SliderControl label="Saturation" v-model="saturation" />
      <SliderControl label="Lightness" v-model="lightness" />
    </div>

    <div v-if="isLoading">Loading...</div>

    <div v-else class="swatch-grid">
      <Swatch v-for="color in colorsToDisplay" :key="color.hue" :color="color" />
    </div>
  </main>
</template>

<style scoped>
#app {
  margin: 0 auto;
  padding: 0 32px;
}

.controls {
  margin: 2rem 0;
}

.swatch-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: flex-start;
}
</style>
