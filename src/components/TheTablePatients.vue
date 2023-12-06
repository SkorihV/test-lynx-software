<script>
export default {
  name: "tablePatient",
  props: {
    tableHeaders: {
      type: Array,
      required: true
    },
    tableData: {
      type: Array,
    }
  },
  methods: {
    getColor(item) {
      return item?.color?.length ? item.color : null
    }
  }
}
</script>

<template>
  <v-container fluid>
    <v-data-table
      :headers="tableHeaders"
      :items="tableData"
      fixed-header
      disable-pagination
    >
      <template v-slot:item="{item}">
        <tr :style="{backgroundColor: getColor(item)}">
            <td>{{item.IDS}}</td>
            <td>{{item.registration}}</td>
            <td>{{item.cardNumber}}</td>
            <td>{{item.patientName}}</td>
            <td>{{item.customer}}</td>
        </tr>
      </template>
    </v-data-table>
  </v-container>

</template>

<style scoped>

</style>
