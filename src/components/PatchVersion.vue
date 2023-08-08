<script setup lang="ts">
import Tag from "./Tag.vue";
const props = defineProps<{
  version: string;
}>();

const colors: Record<string, string> = {
  "4.5": "rgb(228, 101, 124)",
  "5.15": "rgb(127, 15, 170)",
  "5.45": "rgb(144, 103, 173)",
  "6.45": "rgb(191, 120, 19)",
};
</script>

<template>
  <Tag :color="colors[props.version]">
    {{ props.version }}
  </Tag>
</template>
