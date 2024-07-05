<script setup>
defineProps({
  name: String,
  company: String,
  phone: String,
  decrementOrganizations: Function,
});
const emit = defineEmits(["sendOrganizationId"]);



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
  top: 20px;
  right: 10px;
  cursor: pointer;
}
</style>
