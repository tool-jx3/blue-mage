<script setup lang="ts">
import { computed } from "vue";
import { spells, learnedByNo, indexByNo } from "../lib/spell";
import type { FilterTypes, SpellStatusArray } from "@/lib/interface";
import SpellItem from "./SpellItem.vue";

const props = defineProps<{
  filterTypes: FilterTypes;
  filterLevel: number;
  orderByLevel: boolean;
  spellStatus: SpellStatusArray;
}>();
const emit = defineEmits<{
  (e: "change", i: number, status: boolean): void;
}>();

const showSpells = computed(() => {
  const filtered = spells.filter(
    (spell, index) =>
      !props.spellStatus[index] &&
      spell.level <= props.filterLevel &&
      spell.method.some((m) => props.filterTypes[m.type])
  );
  if (props.orderByLevel) {
    filtered.sort((a, b) => a.level - b.level);
  }

  return filtered;
});
</script>

<template>
  <main class="spell-inst">
    <h3>可学习技能列表</h3>
    <p v-if="showSpells.length === 0">当前条件下暂无可学习的技能</p>
    <spell-item
      v-for="spell in showSpells"
      :key="spell.no"
      :spell="spell"
      :learned="learnedByNo(props.spellStatus, spell.no)"
      @change="emit('change', indexByNo(spell.no), $event)"
    />
  </main>
</template>

<style>
.spell-inst h3 {
  margin-top: 0;
}
</style>
