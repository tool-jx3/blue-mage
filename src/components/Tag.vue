<script setup lang="ts">
const props = defineProps<{
  color: string;
}>();
</script>

<template>
  <span class="tag" :style="{ backgroundColor: props.color }">
    <slot></slot>
  </span>
</template>

<style scoped>
.tag {
  display: inline-flex;
  padding: 0 5px;
  line-height: 20px;
  font-size: 0.875rem;
  color: #000;
  font-weight: bold;
  border-radius: 5px;
  align-items: center;
}

.tag:not(:first-child) {
  margin-left: 5px;
}
</style>
