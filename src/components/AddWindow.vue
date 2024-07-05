<script setup>
import { defineProps, defineEmits, ref } from "vue";
import { onClickOutside } from "@vueuse/core";
defineProps({
  isOpen: Boolean,
  isFilled:Boolean,
});
const emit = defineEmits(["modalClose"]);
const target = ref(null);
onClickOutside(target, () => emit("modalClose"));
</script>

<template>
  <div v-if="isOpen" class="modal-background">
    <div class="modal" ref="target">
      <p>Добавить организацию</p>
      <div class="input-container">
        <input class="modal-input" placeholder="Название" type="text" /><input
          class="modal-input"
          placeholder="Номер телефона"
          type="text"
        /><input class="modal-input" placeholder="ФИО директора" type="text" />
      </div>
      <div class="button-wrapper">
        <button  @click="emit('modalClose')" class="modal-button">
          Отмена</button
        ><button :disabled="!isFilled" @click="emit('modalClose')" class="modal-button">
          Ок
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal {
  display: flex;
  position: relative;
  margin: auto;
  width: 450px;
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
  width: 350px;
  height: 36px;
  margin: 0.5rem 0;
}
.modal-button {
  margin: 0 0.5rem;
}
.input-container{
  display: flex;
  flex-direction: column;
}
</style>
