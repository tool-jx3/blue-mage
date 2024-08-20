<script setup lang="ts">
import { computed, ref } from "vue";
import {
  spells,
  learnedByNo,
  indexByNo,
  type Spell,
  renderSpellMethod,
} from "../lib/spell";
import type { FilterTypes, SpellStatusArray } from "@/lib/interface";
import SpellItem from "./SpellItem.vue";

const props = defineProps<{
  filterTypes: FilterTypes;
  filterLevel: number;
  filter: string;
  orderByLevel: boolean;
  spellStatus: SpellStatusArray;
}>();
const emit = defineEmits<{
  (e: "change", i: number, status: boolean): void;
  (e: "clearFilter"): void;
}>();

const notLearnedOnly = ref(true);

type Mode = "search" | "notLearned" | "all";
const mode = computed<Mode>(() => {
  if (props.filter) {
    return "search";
  } else if (notLearnedOnly.value) {
    return "notLearned";
  } else {
    return "all";
  }
});

const filters: Record<Mode, (spell: Spell, index: number) => boolean> = {
  search: (spell) => {
    return (
      spell.spell.includes(props.filter) ||
      spell.method.some((m) => renderSpellMethod(m).includes(props.filter))
    );
  },
  notLearned: (spell, index) => {
    return (
      !props.spellStatus[index] &&
      spell.level <= props.filterLevel &&
      spell.method.some((m) => props.filterTypes[m.type])
    );
  },
  all: (spell) => {
    return (
      spell.level <= props.filterLevel &&
      spell.method.some((m) => props.filterTypes[m.type])
    );
  },
};
const showSpells = computed(() => {
  const filtered = spells.filter(filters[mode.value]);
  if (props.orderByLevel) {
    filtered.sort((a, b) => a.level - b.level);
  }

  return filtered;
});

const allLearned = computed(() =>
  spells.every((_, i) => !!props.spellStatus[i])
);
</script>

<template>
  <main>
    <div class="notice">
      <template v-if="mode === 'notLearned'">
        {{
          showSpells.length
            ? "展示未學習技能"
            : allLearned
            ? "恭喜，您已經掌會了當前版本的所有技能"
            : "當前條件下暫無可學習的技能"
        }}，
        <a href="javascript:void(0)" @click="notLearnedOnly = false">
          切換至所有技能
        </a>
      </template>
      <template v-else-if="mode === 'all'">
        展示所有技能，
        <a href="javascript:void(0)" @click="notLearnedOnly = true">
          切換至未學習技能
        </a>
      </template>
      <template v-else>
        展示包含“{{ props.filter }}”的技能（{{ showSpells.length }} 個），
        <a href="javascript:void(0)" @click="emit('clearFilter')">
          清空搜索條件
        </a>
      </template>
    </div>
    <spell-item
      v-for="spell in showSpells"
      :key="spell.no"
      :spell="spell"
      :learned="learnedByNo(props.spellStatus, spell.no)"
      @change="emit('change', indexByNo(spell.no), $event)"
    />
  </main>
</template>

<style scoped>
.notice {
  border-bottom: 2px solid #ffbe31;
  padding-bottom: 10px;
  margin-bottom: 10px;
  line-height: 32px;
}

.notice a {
  color: #ffbe31;
  text-decoration: none;
}

.notice a:hover {
  border-bottom: 1px solid #ffbe31;
}
</style>
