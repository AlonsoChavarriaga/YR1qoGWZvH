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

{ console.log('color', props.color.hsl) }
</script>
  
<style scoped>
.swatch {
    width: 80px;
    border: 1px solid #ddd;
    overflow: hidden;
    border-radius: 4px;
}

.color-block {
    height: 60px;
}

.color-info {
    padding: 2px;
    font-size: 14px;
    text-align: center;
}

.color-name {
    display: block;
    white-space: nowrap;
}
</style>
  