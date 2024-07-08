<script setup>
import { defineProps } from "vue";
defineProps({
  changeInput: Function,
  openWindow: Function,
});
</script>

<template>
  <header class="header">
    <div class="search-wrapper">
      <input
        placeholder="Найти по ФИО"
        class="search"
        type="search"
        @input="changeInput"
      />
    </div>
    <button @click="openWindow">Добавить</button>
  </header>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
}
.search {
  width: 400px;
  margin-right: 1.5rem;
  height: 36px;
  border-radius: 4px;
  border: none;
  outline: transparent;
  padding-left: 1rem;
}
.search::placeholder {
  color: #acacac;
}
.search-wrapper {
  position: relative;
}
.search-clean {
  position: absolute;
  cursor: pointer;
  top: 10px;
  right: 30px;
}
@media (max-width: 570px) {
  .search {
    width: 200px;
  }
}
</style>
