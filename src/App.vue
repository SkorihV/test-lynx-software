<template>
  <v-app>
    <v-app-bar
      app
      color="green lighten-2"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Lynx software Logo"
          class="shrink mr-2"
          contain
          src="@/assets/logo.png"
          transition="scale-transition"
          width="40"
        />
        <div class="text-h4">Lynx software</div>

      </div>
    </v-app-bar>
    <v-main class="d-flex flex-column">
      <v-container class="d-flex">
        <v-navigation-drawer
          :mini-variant="isMenuMini"
          mini-variant-width="70px"
        >
          <filters-aside
            @changeDataFilters="setFilteredData"
            :customerNames="customerNames"
            @toggleMini="toggleMini"
          />
        </v-navigation-drawer>
        <v-container>
          <registration-date
            @inputTimestamp="setTimestampFilteredData"
          />
      <table-patient
        v-if="isShowTable"
        :table-data="filteredDataTable"
        :table-headers="tableHeaders" />

        </v-container>
      </v-container>

    </v-main>
  </v-app>
</template>

<script>

import RegistrationDate from "@/components/registration-date.vue";
import FiltersAside from "@/components/filters-aside.vue";
import tablePatient from "@/components/table-patient.vue";
import {ALL_CUSTOMER} from "@/constants";

export default {
  name: 'App',

  components: {
    tablePatient,
    FiltersAside,
    RegistrationDate
  },

  data: () => ({
    tableHeaders:[],
    tableData:[],
    filterData: {},
    timestampDateFilters: {
      from: null,
      to: null
    },
    isMenuMini: false
  }),
  methods: {
    setFilteredData(data) {
      this.filterData = data
    },
    setTimestampFilteredData(data) {
      this.timestampDateFilters.from = data.from
      this.timestampDateFilters.to = data.to
    },
    checkedExist(value){
      return value !== null && value !== undefined;
    },
    checkedFieldInFilter(item, fieldName) {
      if (!this.checkedExist(this.filterData[fieldName])) return true
      return item[fieldName].toString().toLowerCase().indexOf(this.filterData[fieldName].toString().toLowerCase()) !== -1
    },
    getTimestamp(date) {
      if (!date) return null
      const [day, month, year] = date.split('.')
      const dateFormatted =  `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
      const newDate = new Date(dateFormatted)
      return newDate.getTime()
    },
    toggleMini() {
      this.isMenuMini = !this.isMenuMini
    }
  },
  computed: {
    isShowTable() {
      return Boolean(this.tableData.length) && Boolean(this.tableHeaders.length)
    },
    filteredDataTableInTime() {
      if (this.timestampDateFilters.from === null || this.timestampDateFilters.to === null) return []

      return this.tableData.filter(item => {
        let timestampItem = this.getTimestamp(item.registration)
        return timestampItem >= this.timestampDateFilters.from && timestampItem <= this.timestampDateFilters.to
      })
    },
    filteredDataTable() {
      return this.filteredDataTableInTime.filter(item => {
        return this.checkedFieldInFilter(item, 'IDS', )
        && this.checkedFieldInFilter(item, 'patientName', )
        && this.checkedFieldInFilter(item, 'customer', )
      })
    },
    customerNames() {
      const names = new Set()
      names.add(ALL_CUSTOMER)
      this.tableData.map(item => {
        if (Boolean(item.customer?.toString().length)) {
          names.add(item.customer)
        }
      })

      return Array.from(names)
    }
  },
  async mounted() {
    await fetch("http://localhost:5000/table-headers")
      .then(res => res.json())
      .then(data => this.tableHeaders = data)
    await fetch("http://localhost:5000/table-data")
      .then(res => res.json())
      .then(data => this.tableData = data)
  }
};
</script>
