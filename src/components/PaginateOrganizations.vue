<script setup>
import { defineProps } from "vue";
defineProps({
  pageNumber: Number,
  setNextPage: Function,
  setPrevPage: Function,
  disabledNext: Boolean,
  disabledPrev: Boolean,
});
</script>

<template>
  <footer class="footer">
    <button @click="setPrevPage" :disabled="disabledPrev">Назад</button
    ><span class="page-number">{{ pageNumber }}</span
    ><button @click="setNextPage" :disabled="disabledNext">Вперед</button>
  </footer>
</template>

<style scoped>
.footer {
  display: flex;
  justify-content: flex-end;
  align-items: center;
}
.page-number {
  text-align: center;
  padding: 0 1rem;
}
</style>
