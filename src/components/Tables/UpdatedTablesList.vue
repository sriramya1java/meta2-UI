<template>
  <div>
    <v-container fluid v-if="pathVal === 'UpdatedTablesList'">
      <v-flex xs12 text-left>
        <v-tooltip top>
        <v-btn :to="{ name: 'edittable', params: { tableString: 'new' }, query: {debug: true }}" slot="activator">Create a New Table</v-btn>
        <span>Click to Create a New Table</span>
      </v-tooltip>
      </v-flex>
      <v-flex xs12 text-center class="pa-2">
        <ag-grid-vue style="width: 100%; height: 200px;"
                         class="ag-theme-balham"
                         :gridOptions="gridOptions"
                         :rowDataChanged="onRowDataChanged"
                         :rowSelected="onRowSelected"
                         :rowData="tablesList">
        </ag-grid-vue>
      </v-flex>
      <v-flex xs12 text-right>
        <v-btn :disabled="!checked"  @click='fileDelivery'>
          Create Delivery File
        </v-btn>
      </v-flex>
    </v-container>
    <router-view></router-view>
  </div>
</template>
<script>
  import {AgGridVue} from 'ag-grid-vue'
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  const router = new VueRouter()
  export default {
    name: 'App',
    data () {
      return {
        columnDefs: null,
        gridOptions: null,
        tableId: null,
        pathVal: '',
        checked: false
      }
    },
    components: {
      'ag-grid-vue': AgGridVue,
      /* 'edit-component': {
        router,
        template: '<router-link to="/edittable">edit table</router-link>'
      }, */
      'delete-component': {
        router,
        template: '<a @click="deleteTable"><i class="fa fa-trash"></i></a>',
        methods: {
          deleteTable () {
            confirm('Do you want to delete the table')
          }
        }
      }
    },
    methods: {
      createColDefs () {
        return [
          {headerName: 'TableID Edit', field: 'edit', cellRenderer: tableCellRenderer, suppressSorting: true, cellStyle: {textAlign: 'left'}},
          {headerName: 'Program ID', field: 'programString', icons: {sortAscending: '<i class="fa fa-sort-alpha-asc"/>', sortDescending: '<i class="fa fa-sort-alpha-desc"/>'}, cellStyle: {textAlign: 'left'}},
          {headerName: 'Dataset ID', field: 'datasetString', icons: {sortAscending: '<i class="fa fa-sort-alpha-asc"/>', sortDescending: '<i class="fa fa-sort-alpha-desc"/>'}, cellStyle: {textAlign: 'left'}},
          /* {headerName: 'Table ID', field: 'tableString', icons: {sortAscending: '<i class="fa fa-sort-alpha-asc"/>', sortDescending: '<i class="fa fa-sort-alpha-desc"/>'}, cellStyle: {textAlign: 'left'}}, */
          {headerName: 'Table Type', field: 'componentString', icons: {sortAscending: '<i class="fa fa-sort-alpha-asc"/>', sortDescending: '<i class="fa fa-sort-alpha-desc"/>'}, cellStyle: {textAlign: 'left'}},
          {headerName: 'Display Label', field: 'displayLabel', icons: {sortAscending: '<i class="fa fa-sort-alpha-asc"/>', sortDescending: '<i class="fa fa-sort-alpha-desc"/>'}, cellStyle: {textAlign: 'left'}},
          {headerName: 'Last Updated By', field: 'lastUpdatedBy', suppressSorting: true, cellStyle: {textAlign: 'left'}},
          {headerName: 'Last Updated', field: 'lastUpdated', suppressSorting: true, cellStyle: {textAlign: 'left'}},
          {headerName: 'Last Delivered', field: 'lastDelivered', suppressSorting: true, cellStyle: {textAlign: 'left'}},
          {headerName: 'Delete', field: 'delete', cellRendererFramework: 'delete-component', suppressSorting: true},
          {headerName: 'Deliver', field: 'deliver', suppressSorting: true, checkboxSelection: true, headerCheckboxSelection: true}
        ]
      },
      onRowDataChanged () {
        Vue.nextTick(() => {
          this.gridOptions.api.sizeColumnsToFit()
        })
      },
      onRowSelected () {
        if (this.gridOptions.api.getSelectedRows().length > 0) {
          console.log(this.gridOptions.api.getSelectedRows().length)
          this.checked = true
        } else {
          console.log(this.gridOptions.api.getSelectedRows().length)
          this.checked = false
        }
      },
      fileDelivery () {
        let tableValues = []
        this.gridOptions.api.getSelectedRows().forEach(x => {
          tableValues.push(x.tableString)
        })
        confirm('Do you want to generate XML for the selected tables? The selected tables are ' + tableValues)
      },
      iconClick (val) {
        // `event` is the native DOM event
        console.log(val)
      }
    },
    created () {
      this.$store.dispatch('getTables')
      this.gridOptions = {
        enableColResize: true,
        enableSorting: true,
        enableFilter: true,
        sortingOrder: ['asc', 'desc'],
        rowSelection: 'multiple',
        suppressRowClickSelection: true,
        columnDefs: this.createColDefs(),
        onGridReady: function (params) {
          params.api.sizeColumnsToFit()
        }
      }
    },
    computed: {
      crumbs () {
        this.pathVal = this.$route.name
        console.log(this.pathVal)
      },
      tablesList () {
        return this.$store.state.tablesList
      }
    },
    watch: {
      crumbs () {
      }
    },
    beforeCreate: function () {
      console.log(this.$baseUrl)
    }
  }
  function tableCellRenderer (params) {
    if (params !== undefined && params !== null) {
      let aTag = document.createElement('a')
      let abc = params.data.tableString
      aTag.setAttribute('href', '#/edittable/' + abc)
      aTag.innerHTML = abc
      console.log(aTag)
      return aTag
    }
  }
</script>
<!-- Add "scoped" attribute to limit css to this component only -->
<style>
  button:disabled {
    cursor: not allowed;
  }
</style>
