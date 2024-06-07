<script setup lang="ts">
import { defineProps } from "vue";
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog";
import QuestionMarkCircle from "@/components/icons/QuestionMarkCircle.vue";
interface Props {
  title: string;
}
defineProps<Props>();
</script>
<template>
  <Dialog>
    <DialogTrigger>
      <QuestionMarkCircle></QuestionMarkCircle>
    </DialogTrigger>
    <DialogContent>
      <DialogHeader>
        <DialogTitle>{{ title }}</DialogTitle>
      </DialogHeader>
      <slot></slot>
    </DialogContent>
  </Dialog>
</template>
