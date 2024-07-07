<script setup>
defineProps({
  name: String,
  company: String,
  phone: String,
  decrementOrganizations: Function,
});
</script>

<template>
  <div class="table-row">
    <div class="table-data">{{ company }}</div>
    <div class="table-data">{{ name }}</div>
    <div class="table-data">{{ phone }}</div>
    <img
      @click="decrementOrganizations"
      class="delete-icon"
      src="../assets/svg/cancel.svg"
      alt="delete"
    />
  </div>
</template>

<style scoped>
.table-row {
  display: table-row;
  position: relative;
  height: 50px;
  width: 100%;
}
.table-data {
  display: table-cell;
  padding: 1rem;
  color: #333;
  border-top: 1px solid #ccc;
}
.delete-icon {
  position: absolute;
  top: 19px;
  right: 10px;
  cursor: pointer;
}
@media (max-width: 690px) {
  .table-data {
    padding: 0.5rem;
  }
}
@media (max-width: 615px) {
  .delete-icon {
    right: 2px;
    top: 16px;
  }
}
@media (max-width: 570px) {
  .delete-icon {
    top: 24px;
  }
}
</style>
