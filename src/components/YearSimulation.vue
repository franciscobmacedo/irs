<script setup lang="ts">
import { defineProps } from "vue";
import { rounded } from "@/lib/utils";
import { Highlight } from "@/components/ui/highlight";
import TaxRanksTable from "@/components/TaxRanksTable.vue";
import InfoDialog from "@/components/InfoDialog.vue";
import { SimulationResult } from "@/lib/types";
interface Props {
  results: SimulationResult;
  year: string;
}
defineProps<Props>();
</script>
<template>
  <section class="space-y-4 my-2">
    <div class="flex gap-2">
      <h3 class="scroll-m-20 text-xl font-semibold tracking-tight">
        Resultados para {{ year }}
      </h3>
      <InfoDialog :title="`Escalões de IRS para ${year}`">
        <TaxRanksTable
          :taxableIncome="results.taxableIncome.value"
          :rankIndex="results.normalRankIndex.value"
          :irsRanks="results.irsRanks"
        ></TaxRanksTable>
      </InfoDialog>
    </div>
    <p class="leading-7 [&:not(:first-child)]:mt-6">
      O rendimento colectável de
      <Highlight>{{ results.taxableIncome }}€</Highlight> encontra-se no
      <Highlight>{{ results.normalRankIndex.value + 1 }}</Highlight> escalão de
      IRS o que resulta numa taxa normal de
      <Highlight>{{ results.normalRate100 }}%</Highlight> aplicada sobre
      <Highlight>{{ results.normalRateIncome }}€</Highlight> ({{
        results.normalRate100
      }}% x {{ results.normalRateIncome }}€ =
      {{ rounded(results.normalRateTax.value) }}€)<span
        v-if="results.averageRate"
      >
        e numa taxa média de
        {{ results.averageRate100 }}% aplicada sobre
        <Highlight>{{ results.averageRateIncome }}€</Highlight> ({{
          results.averageRate100
        }}% x {{ results.averageRateIncome }}€ =
        {{ rounded(results.averageRateTax.value) }}€)</span
      >. O resultado é um imposto a pagar de
      <Highlight>{{ rounded(results.taxToPay.value) }}€</Highlight>.
    </p>
  </section>
</template>
