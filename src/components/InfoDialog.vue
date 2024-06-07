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
defineProps({
  title: { required: false, type: String },
});
</script>
<template>
  <Dialog>
    <DialogTrigger>
      <QuestionMarkCircle></QuestionMarkCircle>
    </DialogTrigger>
    <DialogContent>
      <DialogHeader v-if="title">
        <DialogTitle>{{ title }}</DialogTitle>
      </DialogHeader>
      <slot></slot>
    </DialogContent>
  </Dialog>
</template>
