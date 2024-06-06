<script setup lang="ts">
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { ref, computed } from "vue";
import {
  IRS_RANKS_2023,
  IRS_RANKS_2024_NEW,
  IRS_RANKS_2024_OLD,
} from "./data/irsRanks.ts";
import CurrencyEuro from "@/components/icons/CurrencyEuro.vue";
import { useTax } from "@/composables/tax";
import YearSimulation from "@/components/YearSimulation.vue";
import Table from "@/components/Table.vue";
import Footer from "@/components/Footer.vue";
import { DataItem } from "@/lib/types";

// taxable income
const taxableIncome = ref(10000);
const increaseTaxableIncome = (value: number) => {
  const result = taxableIncome.value + value;
  taxableIncome.value = result < 0 ? 0 : result;
};

const results2024New = useTax(taxableIncome, IRS_RANKS_2024_NEW);
const results2024Old = useTax(taxableIncome, IRS_RANKS_2024_OLD);
const results2023 = useTax(taxableIncome, IRS_RANKS_2023);

const data = computed((): DataItem[] => [
  {
    header: "2023",
    result: results2023,
  },

  {
    header: "2024 (antigo)",
    result: results2024Old,
  },
  {
    header: "2024 (novo)",
    result: results2024New,
  },
]);
</script>

<template>
  <div class="container mx-auto my-24">
    <h1 class="scroll-m-20 text-4xl font-extrabold tracking-tight lg:text-5xl">
      Simulação de IRS nos diferentes anos
    </h1>
    <p class="leading-7 [&:not(:first-child)]:mt-6 text-muted-foreground">
      Foram recentemente aprovadas os <a class="underline underline-offset-2 hover:text-neutral-600" href="https://www.publico.pt/2024/04/19/economia/noticia/irs-baixa-oitavo-escaloes-ganha-menos-ha-novo-alivio-2087600" target="_blank">novos escalões de IRS</a>. Este simulador permite comparar os impostos a pagar para os anos de 2023, 2024 (antigo) e 2024 (novo) com base
      num rendimento coletável (anual). Para saber o seu rendimento coletável, consulte <a class="underline underline-offset-2 hover:text-neutral-600" href="https://www.comparaja.pt/blog/escaloes-irs#:~:text=O%20que%20%C3%A9%20o%20rendimento,da%20tua%20categoria%20de%20rendimentos." target="_blank">este artigo</a>.
    </p>
    <div class="flex flex-col items-start justify-start py-12 gap-1">
      <span
        class="text-sm font-medium leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70"
      >
        Rendimento coletável (anual)
      </span>
      <div class="flex flex-col md:flex-row gap-2 items-start md:items-center">
        <div class="relative w-full max-w-sm items-center">
          <Input
            type="number"
            placeholder="Rendimento coletável (€)"
            class="pl-10 w-96"
            v-model="taxableIncome"
          />
          <span
            class="absolute start-0 inset-y-0 flex items-center justify-center px-2"
          >
            <CurrencyEuro class="size-6 text-muted-foreground"></CurrencyEuro>
          </span>
        </div>
        <div class="flex gap-1">
          <Button
            variant="outline"
            @click="increaseTaxableIncome(-5000)"
            :disabled="taxableIncome <= 0"
            >- 5000€</Button
          >
          <Button variant="outline" @click="increaseTaxableIncome(5000)"
            >+ 5000€</Button
          >
        </div>
      </div>
    </div>

    <Table :data="data"></Table>
    <div class="my-12">
      <p class="leading-7 [&:not(:first-child)]:mt-6">
        Resultados descritivos da simulação
      </p>
      <div class="flex flex-col gap-4">
        <YearSimulation
          :results="results2023"
          :year="'2023'"
          :taxableIncome="taxableIncome"
          :irsRanks="IRS_RANKS_2023"
        />
        <YearSimulation
          :results="results2024Old"
          :year="'2024 (antigo)'"
          :taxableIncome="taxableIncome"
          :irsRanks="IRS_RANKS_2024_OLD"
        />
        <YearSimulation
          :results="results2024New"
          :year="'2024 (novo)'"
          :taxableIncome="taxableIncome"
          :irsRanks="IRS_RANKS_2024_NEW"
        />
      </div>
    </div>
  </div>
  <Footer></Footer>
</template>
