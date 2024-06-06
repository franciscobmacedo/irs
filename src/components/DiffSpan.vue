<script setup lang="ts">
import { DataItem, SimulationResult } from "@/lib/types";
import { computed } from "vue";
import { rounded } from "@/lib/utils";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/tooltip";
import QuestionMarkCircle from "@/components/icons/QuestionMarkCircle.vue";

// taxable income
interface Props {
  data: DataItem[];
  index: number;
  field: keyof SimulationResult;
}

const props = defineProps<Props>();

const value = computed((): number | null => {
  const value = props.data[props.index].result[props.field].value;
  if (typeof value !== "number") return null;
  return value;
});

const previousValue = computed((): number | null => {
  if (props.index === 0) return null;
  const value = props.data[props.index - 1].result[props.field].value;
  if (typeof value !== "number") return null;
  return value;
});

const diff = computed((): number | null => {
  if (value.value === null || previousValue.value === null) return null;
  return value.value - previousValue.value;
});

const diffPercentage = computed((): number | null => {
  if (value.value === null || previousValue.value === null) return null;
  return rounded((value.value / previousValue.value - 1) * 100);
});
</script>

<template>
  <TooltipProvider :delayDuration=100>
    <Tooltip>
      <TooltipTrigger as-child>
        <span class="font-semibold gap-2 flex items-center">
          <span
            class="text-sm"
            v-if="diff"
            :class="diff > 0 ? 'text-red-400' : 'text-green-400'"
            >{{ diff > 0 ? `+${rounded(diff)}€` : `-${rounded(-diff)}€` }}
          </span>
          <span v-if="diffPercentage" class="text-xs text-muted-foreground">
            {{ diffPercentage }}%</span
          >

          <QuestionMarkCircle v-if="diffPercentage"></QuestionMarkCircle>
        </span>
      </TooltipTrigger>
      <TooltipContent>
        <p>diferença entre esta coluna e a anterior</p>
      </TooltipContent>
    </Tooltip>
  </TooltipProvider>
</template>