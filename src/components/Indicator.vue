<script setup lang="ts">
const props = defineProps<{
  checked: boolean;
  bordered?: boolean;
}>();
const emit = defineEmits<{
  (e: "change", value: boolean): void;
}>();
</script>

<template>
  <div
    class="indicator"
    :class="{
      checked: props.checked,
      bordered: props.bordered,
    }"
    @click="emit('change', !props.checked)"
  ></div>
</template>

<style scoped>
.indicator {
  display: block;
  width: 8px;
  height: 8px;
  content: "";
  background: #222;
  border: 2px solid #222;
}

.indicator.bordered {
  box-shadow: 0 0 1px #ffbe31;
}

.indicator.checked {
  box-shadow: none;
  background: #ffbe31;
}
</style>
