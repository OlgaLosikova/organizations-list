<script setup>
import { reactive, ref } from "vue";
import Header from "./components/Header.vue";
import TableRow from "./components/TableRow.vue";
import PaginateOrganizations from "./components/PaginateOrganizations.vue";
import AddWindow from "./components/AddWindow.vue";
import data from "../src/assets/data.json";

const window = reactive({
  isWindowOpened: false,
});
const filters = reactive({
  nameDirection: "",
  companyDirection: "",
  searchValue: "",
  isFiltred: false,
});
const page = reactive({
  pageNumber: 1,
  disabledNext: false,
  disabledPrev: true,
  itemIndex: 0,
});

const changedOrganizations = ref(data);
const filtredOrganizations = ref([]);
const organizationsPage = ref(
  changedOrganizations.value.slice(page.itemIndex, page.itemIndex + 10)
);

const openWindow = () => {
  window.isWindowOpened = true;
};
const closeModal = () => {
  window.isWindowOpened = false;
};
const submitHandler = (newOrganization) => {
  changedOrganizations.value.push(newOrganization);
  filters.isFiltred && filtredOrganizations.value.push(newOrganization);
  rotatePage();
  checkListLenth();
};
const changeInput = (e) => {
  filters.searchValue = e.target.value;
  if (filters.searchValue.length > 2 || filters.searchValue === "") {
    filtredOrganizations.value = changedOrganizations.value.filter((obj) =>
      obj.name.toLowerCase().includes(filters.searchValue.toLowerCase())
    );
    filters.searchValue === ""
      ? (filters.isFiltred = false)
      : (filters.isFiltred = true);
    goToFirstPage();
    rotatePage();
  }
};
const goToFirstPage = () => {
  page.itemIndex = 0;
  page.pageNumber = 1;
  page.disabledPrev = true;
  filtredOrganizations.value.length > 10
    ? (page.disabledNext = false)
    : (page.disabledNext = true);
};
const rotatePage = () => {
  organizationsPage.value = (
    filters.isFiltred ? filtredOrganizations.value : changedOrganizations.value
  ).slice(page.itemIndex, page.itemIndex + 10);
};
const setNextPage = () => {
  page.itemIndex += 10;
  page.pageNumber++;
  page.disabledPrev = false;
  rotatePage();
  checkListLenth();
};
const setPrevPage = () => {
  page.itemIndex -= 10;
  page.pageNumber--;
  page.disabledNext = false;
  rotatePage();
  if (page.itemIndex === 0) {
    page.disabledPrev = true;
  }
  checkListLenth();
};
const checkListLenth = () => {
  if (
    page.itemIndex >=
    (filters.isFiltred
      ? filtredOrganizations.value.length - 10
      : changedOrganizations.value.length - 10)
  ) {
    page.disabledNext = true;
  } else page.disabledNext = false;
};
const sortOrganizations = () => {
  const sortData = filters.isFiltred
    ? filtredOrganizations.value
    : changedOrganizations.value;
  if (filters.nameDirection === "DESC") {
    sortData.sort((a, b) => (a.name < b.name ? 1 : -1));
  } else if (filters.nameDirection === "ASC") {
    sortData.sort((a, b) => (a.name > b.name ? 1 : -1));
  } else if (filters.companyDirection === "DESC") {
    sortData.sort((a, b) => (a.company < b.company ? 1 : -1));
  } else if (filters.companyDirection === "ASC") {
    sortData.sort((a, b) => (a.company > b.company ? 1 : -1));
  } else sortData.sort((a, b) => a.id - b.id);
  rotatePage();
};
const sortName = () => {
  filters.companyDirection = "";
  filters.nameDirection === ""
    ? (filters.nameDirection = "DESC")
    : filters.nameDirection === "DESC"
    ? (filters.nameDirection = "ASC")
    : filters.nameDirection === "ASC" && (filters.nameDirection = "");
  sortOrganizations();
};
const sortCompany = () => {
  filters.nameDirection = "";
  filters.companyDirection === ""
    ? (filters.companyDirection = "DESC")
    : filters.companyDirection === "DESC"
    ? (filters.companyDirection = "ASC")
    : filters.companyDirection === "ASC" && (filters.companyDirection = "");
  sortOrganizations();
};

const decrementOrganizations = (id) => {
  changedOrganizations.value = changedOrganizations.value.filter(
    (obj) => obj.id !== id
  );
  filters.isFiltred &&
    (filtredOrganizations.value = filtredOrganizations.value.filter(
      (obj) => obj.id !== id
    ));
  rotatePage();
  checkListLenth();
};
</script>

<template>
  <Header :changeInput="changeInput" :openWindow="openWindow" />
  <main class="main">
    <div class="table-container">
      <div class="table">
        <div class="table-title">
          <a @click="sortCompany" href="#">Название</a
          ><img
            class="sort-icon"
            v-if="filters.companyDirection === 'ASC'"
            src="../src/assets/svg/arrow_drop_up.svg"
            alt="arrow-up"
          /><img
            class="sort-icon"
            v-if="filters.companyDirection === 'DESC'"
            src="../src/assets/svg/arrow_drop_down.svg"
            alt="arrow-down"
          />
        </div>
        <div class="table-title">
          <a href="#" @click="sortName">ФИО директора</a
          ><img
            class="sort-icon"
            v-if="filters.nameDirection === 'ASC'"
            src="../src/assets/svg/arrow_drop_up.svg"
            alt="arrow-up"
          /><img
            class="sort-icon"
            v-if="filters.nameDirection === 'DESC'"
            src="../src/assets/svg/arrow_drop_down.svg"
            alt="arrow-down"
          />
        </div>
        <div class="table-title">Номер телефона</div>
        <TableRow
          v-for="item in organizationsPage"
          :key="item.id"
          :name="item.name"
          :company="item.company"
          :phone="item.phone"
          :decrementOrganizations="() => decrementOrganizations(item.id)"
        />
      </div>
    </div>
    <AddWindow
      :isFilled="window.isWindowFilled"
      :isOpen="window.isWindowOpened"
      @modalClose="closeModal"
      @onSubmit="submitHandler"
    />
  </main>
  <PaginateOrganizations
    v-if="
      filters.isFiltred
        ? filtredOrganizations.length >= 10
        : changedOrganizations.length >= 10
    "
    :setPrevPage="setPrevPage"
    :setNextPage="setNextPage"
    :pageNumber="page.pageNumber"
    :disabledNext="page.disabledNext"
    :disabledPrev="page.disabledPrev"
  />
</template>

<style scoped>
.main {
  margin: 1.5rem 0;
}
.table {
  display: table;
  border-collapse: collapse;
  width: 100%;
}
.table-container {
  display: flex;
  background-color: rgba(255, 255, 255, 0.8);
}

.table-title {
  display: table-cell;
  padding: 1rem;
  color: white;
  background-color: #646cff;
}
.sort-icon {
  position: relative;
  top: 5px;
}
@media (max-width: 570px) {
  .table-title {
    padding: 0.5rem;
  }
}
</style>
