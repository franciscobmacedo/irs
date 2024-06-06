<script setup lang="ts">
import { defineProps } from "vue";
import { rounded } from "@/lib/utils";
import { Highlight } from "@/components/ui/highlight";
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog";
import QuestionMarkCircle from "@/components/icons/QuestionMarkCircle.vue";
import TaxRanksTable from "@/components/TaxRanksTable.vue";
import { IRSRank, SimulationResult } from "@/lib/types";
interface Props {
  results: SimulationResult;
  taxableIncome: number;
  year: string;
  irsRanks: IRSRank[];
}
defineProps<Props>();
</script>
<template>
  <section class="space-y-4 my-2">
    <div class="flex gap-2">
      <h3 class="scroll-m-20 text-xl font-semibold tracking-tight">
        Resultados para {{ year }}
      </h3>

      <Dialog>
        <DialogTrigger>
          <QuestionMarkCircle :size="5"></QuestionMarkCircle>
        </DialogTrigger>
        <DialogContent>
          <DialogHeader>
            <DialogTitle>Escalões de IRS para {{year}}</DialogTitle>
          </DialogHeader>
          <TaxRanksTable
            :taxableIncome="taxableIncome"
            :rankIndex="results.normalRankIndex.value"
            :irsRanks="irsRanks"
          ></TaxRanksTable>
        </DialogContent>
      </Dialog>
    </div>
    <p class="leading-7 [&:not(:first-child)]:mt-6">
      O rendimento colectável de
      <Highlight>{{ taxableIncome }}€</Highlight> encontra-se no
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
    <!-- <div class="flex my-4">
    
    <Table class="max-w-3xl">
      <TableCaption>Escalões do IRS</TableCaption>
      <TableHeader>
        <TableRow>
          <TableHead>
            Rendimento colectável
          </TableHead>
          <TableHead>Taxa Normal (%)</TableHead>
          <TableHead>Taxa Média (%)</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        <TableRow v-for="(taxRank, index) in irsRanks" :key="index"
          :class="index === results.normalRankIndex && 'bg-muted/70'">
          <TableCell class="font-medium">
            <span v-if="getMin(index)">
              <span v-if="taxRank.max">
                De mais de {{ getMin(irsRanks.indexOf(taxRank)) }} -
                até {{ taxRank.max }}
              </span>
              <span v-else>
                Superior a {{ getMin(irsRanks.indexOf(taxRank)) }}
              </span>
            </span>
            <span v-else>
              Até {{ taxRank.max }}
            </span>

          </TableCell>
          <TableCell>{{ formatNumber(taxRank.normalRate, 2) }}</TableCell>
          <TableCell>{{ taxRank.averageRate ? formatNumber(taxRank.averageRate, 3) : "-" }}</TableCell>
        </TableRow>
      </TableBody>
    </Table>

  </div> -->
  </section>
</template>
