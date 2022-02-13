<template>
  <h3 class="title">
    <slot></slot>
  </h3>
</template>

<style scoped>
.title {
  margin-top: 20px;
  margin-bottom: 20px;
}
.title:first-child {
  margin-top: 0;
}
</style>
