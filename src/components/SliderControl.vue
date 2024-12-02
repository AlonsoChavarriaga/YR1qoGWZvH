<template>
    <div class="slider-control">
        <label :for="label">{{ label }}: {{ modelValue }}%</label>
        <input type="range" :id="label" min="0" max="100" v-model.number="localValue" />
    </div>
</template>
  
<script setup lang="ts">
import { defineProps, defineEmits, computed } from 'vue';

const props = defineProps<{
    label: string;
    modelValue: number;
}>();

const emits = defineEmits<{
    (e: 'update:modelValue', value: number): void;
}>();

const localValue = computed<number>({
    get: () => props.modelValue,
    set: (value) => emits('update:modelValue', value),
});
</script>
  