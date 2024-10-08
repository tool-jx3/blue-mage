<script setup lang="ts">
import type { FilterTypes } from "@/lib/interface";
import type { SpellType } from "@/lib/spell";
import Title from "./Title.vue";
import Indicator from "./Indicator.vue";

const props = defineProps<{
  filterTypes: FilterTypes;
  filterLevel: number;
  orderByLevel: boolean;
}>();

const emit = defineEmits<{
  (e: "levelChange", val: number): void;
  (e: "typeChange", val: SpellType, checked: boolean): void;
  (e: "orderChange", val: boolean): void;
}>();

const handleInput = (e: Event) => {
  let val = +(e?.target as any).value;
  if (isNaN(val)) val = 80;
  emit("levelChange", val);
};

const handleClick = (type: SpellType, checked: boolean) => {
  emit("typeChange", type, !checked);
};

const handleOrder = (order: boolean) => {
  emit("orderChange", !order);
};
</script>

<template>
  <div class="wrap">
    <Title>角色等級</Title>
    <div class="level">
      <input
        type="number"
        max="80"
        min="1"
        :value="props.filterLevel"
        @input="handleInput"
      />
      <div
        class="order"
        :class="{ checked: props.orderByLevel }"
        @click="handleOrder(props.orderByLevel)"
      >
        <Indicator :checked="props.orderByLevel" bordered />
        按等級排序
      </div>
    </div>

    <Title>學習途徑過濾</Title>
    <ul>
      <li
        v-for="(checked, type, i) in filterTypes"
        :key="type"
        class="type"
        :class="{
          lighter: i % 2 === 0,
        }"
        @click="handleClick(type, checked)"
      >
        <img :src="`icons/type_${type}.png`" />
        <Indicator :checked="checked" />
      </li>
    </ul>
  </div>
</template>

<style scoped>
.wrap {
  user-select: none;
  margin-bottom: 20px;
}

.wrap input {
  padding: 0 10px;
  line-height: 32px;
  background: #333;
  color: #fff;
  border: 1px solid #333;
  border-radius: 16px;
}

.level {
  display: flex;
}

.order {
  display: flex;
  align-items: center;
  margin-left: 10px;
  cursor: pointer;
}

.order .indicator {
  margin-right: 10px;
}

.wrap ul {
  display: flex;
  flex-wrap: wrap;
  margin: 0;
  padding: 0;
  flex-shrink: 0;
  list-style: none;
}

.type {
  position: relative;
  padding: 10px 0 6px;
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
}

.type img {
  width: 32px;
  height: 32px;
}

.type.lighter {
  background: #373737;
}

.type .indicator {
  margin-top: 10px;
}
</style>
