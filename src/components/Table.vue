<script setup lang="ts">
import { rounded } from "@/lib/utils";
import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "./ui/table";
import { DataItem } from "@/lib/types";
import DiffSpan from '@/components/DiffSpan.vue';
// taxable income
interface Props {
  data: DataItem[];
}

defineProps<Props>();

</script>

<template>
  <Table class="max-w-5xl">
    <TableCaption>Resultados da Simulação</TableCaption>
    <TableHeader>
      <TableRow>
        <TableHead> </TableHead>
        <TableHead v-for="item in data" :key="item.header">{{
          item.header
        }}</TableHead>
      </TableRow>
    </TableHeader>
    <TableBody>
      <TableRow>
        <TableCell class="font-medium"> Escalão de IRS </TableCell>
        <TableCell v-for="item in data" :key="item.header">{{
          item.result.normalRankIndex.value + 1
        }}</TableCell>
      </TableRow>
      <TableRow>
        <TableCell class="font-medium"> Taxa normal </TableCell>
        <TableCell v-for="item in data" :key="item.header"
          >{{ item.result.normalRate100 }}%</TableCell
        >
      </TableRow>
      <TableRow>
        <TableCell class="font-medium">
          Rendimento sujeito a taxa normal
        </TableCell>
        <TableCell v-for="item in data" :key="item.header"
          >{{ item.result.normalRateIncome }}€</TableCell
        >
      </TableRow>
      <TableRow>
        <TableCell class="font-medium"> Taxa média </TableCell>
        <TableCell v-for="item in data" :key="item.header"
          >{{ item.result.averageRate100 }}%</TableCell
        >
      </TableRow>
      <TableRow>
        <TableCell class="font-medium">
          Rendimento sujeito a taxa média
        </TableCell>
        <TableCell v-for="item in data" :key="item.header"
          >{{ item.result.averageRateIncome }}€</TableCell
        >
      </TableRow>
      <TableRow class="font-bold">
        <TableCell> Impostos a pagar </TableCell>
        <TableCell v-for="(item, index) in data" :key="item.header"
          >{{ rounded(item.result.taxToPay.value) }}€
          <DiffSpan :data="data" :index="index" field="taxToPay" />
        </TableCell>
      </TableRow>
    </TableBody>
  </Table>
</template>
