<script>
export default {
  name: "registration-date",
  emits:['inputTimestamp'],
  data() {
    return {
      dateFrom: this.currentDate(),
      dateTo: this.currentDate(),
      dateFormattedFrom: this.currentDateFormatted(),
      dateFormattedTo: this.currentDateFormatted(),
      menu1: false,
      menu2: false,
    }
  },
  methods: {
    formatDate (date) {
      if (!date) return null
      const [year, month, day] = date.split('-')
      return `${day}.${month}.${year}`
    },
    parseDate (date) {
      if (!date) return null
      const [day, month, year] = date.split('.')
      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
    },
    currentDate() {
      return (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10)
    },
    currentDateFormatted() {
      return this.formatDate((new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10))
    },
    inputData(){
      console.log({from: this.timestampFrom, to: this.timestampTo})
      this.$emit('inputTimestamp', {from: this.timestampFrom, to: this.timestampTo})
    },
  },
  computed: {
    timestampFrom () {
      const date = new Date(this.dateFrom);
      return date.getTime()
    },
    timestampTo () {
      const date = new Date(this.dateTo);
      return date.getTime()
    },
    computedDateFormattedFrom () {
      return this.formatDate(this.dateFrom)
    },
    computedDateFormattedTo () {
      return this.formatDate(this.dateTo)
    },
  },
}
</script>

<template>
  <v-card class="d-flex align-center px-4">
    <div class="text-subtitle-1 text--black pr-4">Дата регистрации</div>
    <v-row>
      <v-col
        cols="12"
        lg="2"
      >
        <v-menu
          ref="menu1"
          v-model="menu1"
          :close-on-content-click="false"
          transition="scale-transition"
          offset-y
          max-width="290px"
          min-width="auto"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="computedDateFormattedFrom"
              label="c:"
              persistent-hint
              prepend-icon="mdi-calendar"
              v-bind="attrs"
              @blur="dateFrom = parseDate(dateFormattedFrom)"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker
            v-model="dateFrom"
            no-title
            @input="menu1 = false"
          ></v-date-picker>
        </v-menu>
      </v-col>
      <v-col
        cols="12"
        lg="2"
      >
        <v-menu
          v-model="menu2"
          :close-on-content-click="false"
          transition="scale-transition"
          offset-y
          max-width="290px"
          min-width="auto"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="computedDateFormattedTo"
              label="по:"
              persistent-hint
              prepend-icon="mdi-calendar"
              readonly
              @blur="dateTo = parseDate(dateFormattedTo)"
              v-bind="attrs"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker
            v-model="dateTo"
            no-title
            @input="menu2 = false"
          ></v-date-picker>
        </v-menu>
      </v-col>
      <v-col cols="12" lg="2">
        <v-btn
          class="ma-2 success"
          color="green lighten-1"
          @click="inputData"
        >
          <v-icon
          color="white">
            mdi-checkbox-marked-circle
          </v-icon>
          Выбрать
        </v-btn>
      </v-col>
    </v-row>
  </v-card>
</template>

<style scoped>

</style>
