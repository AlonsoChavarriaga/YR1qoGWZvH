<template>
    <div class="swatch">
        <div class="color-block" :style="{ backgroundColor: color.hsl }"></div>
        <div class="color-info">
            <span class="color-name">{{ color.name }}</span>
            <span class="rgb-values">{{ color.rgb }}</span>
        </div>
    </div>
</template>
  
<script setup lang="ts">
import { defineProps } from 'vue';

export interface ColorSwatchData {
    hsl: string;
    hue: number;
    name: string;
    rgb: string;
}

const props = defineProps<{
    color: ColorSwatchData;
}>();
</script>
  
<style scoped>
.swatch {
    width: 100px;
    border: 1px solid #ddd;
    overflow: hidden;
    border-radius: 4px;
}

.color-block {
    height: 60px;
}

.color-name {
    font-weight: bold;
    font-size: 12px;
}

.color-info {
    padding: 6px;
    font-size: 10px;
    text-align: center;
}

.color-name,
.rgb-values {
    display: block;
    white-space: nowrap;
}
</style>
  