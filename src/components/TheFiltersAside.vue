<script>
import {ALL_CUSTOMER} from "@/constants";
export default {
  name: "filters-aside",
  emits:['changeDataFilters', 'toggleMini'],
  props:{
    customerNames: {
      type: Array
    }
  },
  data() {
    return {
      rules: {
        number: value => {
          const pattern = /^\d*$/g
          return pattern.test(value) || "Только числа"
        },
        string: value => {
          const pattern = /^[a-zA-Zа-яА-ЯёЁ\s]*$/
          return pattern.test(value) || "Только текст"
        }
      },
      IDS: null,
      patientName: null,
      customer: null,
      timer: null,
      isMini: false
    }
  },
  methods:{
    inputData() {
      const returnData = {
        IDS: this.isExist(this.IDS) ? parseInt(this.IDS) : null,
        patientName: this.isExist(this.patientName) ? this.patientName?.toString() : null,
        customer: this.isExist(this.customer) && this.returnStringValue(this.customer) !== ALL_CUSTOMER ? this.customer?.toString() : null
      }

      clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.$emit('changeDataFilters', returnData)
      }, 1000)

    },
    returnStringValue(value) {
      return value?.toString().trim()
    },
    isExist(value) {
      return Boolean(this.returnStringValue(value)?.length)
    },
    resetFilters() {
      this.IDS = null;
      this.patientName = null;
      this.customer = null;
      this.inputData();
    },
    toggleMini() {
      this.isMini = !this.isMini
      this.$emit('toggleMini')
    },
  },
  watch: {
    IDS() {
      this.inputData()
    },
    patientName() {
      this.inputData()
    },
    customer() {
      this.inputData()
    }
  }
}
</script>

<template>
<div>
    <v-container>
      <div class="
        text-lg-body-1
        d-flex
        justify-lg-space-between
        align-center">
        <span v-show="!isMini">Фильтры</span>
        <v-btn
          class="ma-1 success"
          color="green lighten-1"
          @click="resetFilters"
          v-if="!isMini"
        >
          <v-icon>mdi mdi-eraser</v-icon>
        </v-btn>
        <v-btn
          class="success"
          color="green lighten-1"
          @click="toggleMini"
        >
          <v-icon v-if="isMini">mdi mdi-arrow-right</v-icon>
          <v-icon v-else>mdi mdi-arrow-left</v-icon>
        </v-btn>

      </div>
    </v-container>
    <v-container>
      <v-icon v-if="isMini">mdi mdi-file-document-edit-outline</v-icon>
      <v-text-field
        v-else
        label="IDS"
        v-model="IDS"
        clear-icon="mdi-close-circle"
        clearable
        prepend-icon="mdi mdi-file-document-edit-outline"
        :rules="[rules.number]"
      ></v-text-field>
    </v-container>
    <v-container>
      <v-icon v-if="isMini">mdi mdi-file-account-outline</v-icon>
      <v-text-field
        v-else
        label="Фамилия пациента"
        v-model="patientName"
        clear-icon="mdi-close-circle"
        clearable
        prepend-icon="mdi mdi-file-account-outline"
        :rules="[rules.string]"
      ></v-text-field>
    </v-container>

    <v-container>
      <v-icon v-if="isMini">mdi mdi-account-group</v-icon>
      <v-select
        v-else
        :items="customerNames"
        label="Заказчик"
        v-model="customer"
        prepend-icon="mdi mdi-account-group"
      ></v-select>
    </v-container>
  </div>
</template>

<style scoped>

</style>
