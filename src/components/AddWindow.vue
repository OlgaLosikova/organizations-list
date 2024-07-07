<script setup>
import { defineProps, defineEmits, ref, reactive } from "vue";
import { onClickOutside } from "@vueuse/core";
defineProps({
  isOpen: Boolean,
});
let organizationId = 25;
const newOrganization = reactive({
  name: "",
  company: "",
  phone: "",
});
let isFilled = ref(false);
const emit = defineEmits(["modalClose", "onSubmit"]);
const target = ref(null);
onClickOutside(target, () => emit("modalClose"));
const onSubmit = () => {
  organizationId++;
  emit("onSubmit", {
    name: newOrganization.name,
    company: newOrganization.company,
    phone: newOrganization.phone,
    id: organizationId,
  });
  emit("modalClose");
  newOrganization.name = "";
  newOrganization.company = "";
  newOrganization.phone = "";
  isFilled.value = false;
};
const checkFill = () => {
  newOrganization.company !== "" &&
  newOrganization.name !== "" &&
  newOrganization.phone !== ""
    ? (isFilled.value = true)
    : (isFilled.value = false);
};
</script>

<template>
  <div v-if="isOpen" class="modal-background">
    <div class="modal" ref="target">
      <p>Добавить организацию</p>
      <form class="form" @submit.prevent="onSubmit">
        <div class="input-container">
          <input
            @input="checkFill"
            v-model="newOrganization.company"
            class="modal-input"
            placeholder="Название"
            type="text"
          /><input
            @input="checkFill"
            v-model="newOrganization.phone"
            class="modal-input"
            placeholder="Номер телефона"
            type="text"
          /><input
            @input="checkFill"
            v-model="newOrganization.name"
            class="modal-input"
            placeholder="ФИО директора"
            type="text"
          />
        </div>
        <div class="button-wrapper">
          <button type="button" @click="emit('modalClose')" class="modal-button">
            Отмена</button
          ><button :disabled="!isFilled" type="submit" class="modal-button">
            Ок
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.modal {
  display: flex;
  position: relative;
  margin: auto;
  max-width: 95vw;
  height: 300px;
  padding: 32px 36px 32px 36px;
  border-radius: 8px;
  background-color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #333;
}
.modal-background {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1000;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 60px 15px;
  font-size: 1.5rem;
}
.modal-input {
  height: 36px;
  margin: 0.5rem 0;
  border-radius: 4px;
  border: 1px solid #ccc;
  outline: transparent;
  padding-left: 1rem;
}
.modal-button {
  margin: 0 0.5rem;
}
.form{
  width: 100%;
}
.input-container {
  display: flex;
  flex-direction: column;
}
.button-wrapper {
  display: flex;
  justify-content: center;
}
</style>
