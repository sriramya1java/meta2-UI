<template>
  <v-form ref="form" v-model="valid" lazy-validation>
    <v-layout row wrap align-center>
      <v-flex xs2>
        <v-subheader>Program: <span>*</span></v-subheader>
      </v-flex>
      <v-flex xs4>
        <v-select
          :items="programs"
          label="Select a Program"
          v-model="selectedProgram"
          v-on:change="getComponentsList"
          :rules="selectedProgramRules"></v-select>
      </v-flex>
       <v-flex xs6>
        <span>(* = required)</span>
      </v-flex>
      <v-flex xs2>
        <v-subheader>Component: <span>*</span></v-subheader>
      </v-flex>
      <v-flex xs10>
        <v-select
          :items="components"
          label="Select a Component"
          v-model="selectedComponent"
          v-on:change="getDatasetsList"
          :disabled="selectedProgram.length === 0"
          :rules="selectedComponentRules"></v-select>
      </v-flex>
      <v-flex xs2>
        <v-subheader>Dataset: <span>*</span></v-subheader>
      </v-flex>
      <v-flex xs10>
        <v-select
          :items="datasets"
          label="Select a Dataset"
          :disabled="selectedComponent.length === 0"
          v-model="selectedDataset"
          :rules="selectedDatasetRules"></v-select>
      </v-flex>
      <v-flex xs2>
        <v-subheader>Table ID: <span>*</span></v-subheader>
      </v-flex>
      <v-flex xs10>
        <v-text-field
          label="Enter a Table ID"
          v-model="selectedTableId"
          :rules="selectedTableIdRules"></v-text-field>
      </v-flex>
      <v-flex xs2>
        <v-subheader>Display Label: <span>*</span></v-subheader>
      </v-flex>
      <v-flex xs10>
        <v-text-field
          label="Enter a Display Label"
          v-model="selectedDisplayLabel"
          :rules="selectedDisplayLabelRules"></v-text-field>
      </v-flex>
      <v-flex xs2>
        <v-subheader>Display Description: </v-subheader>
      </v-flex>
      <v-flex xs10>
        <v-textarea
          label="Enter some description."></v-textarea>
      </v-flex>
      <v-flex xs2>
        <v-subheader>Table Universe: </v-subheader>
      </v-flex>
      <v-flex xs8>
        <v-text-field
          label="Table Universe"
          v-model="selectedTableUniverse"></v-text-field>
      </v-flex>
      <v-flex xs2>
        <v-dialog
          v-model="dialog"
          width="1000">
          <v-btn
            slot="activator"
            color="grey lighten-2"
            dark
            icon>
            <v-icon>call_made</v-icon>
          </v-btn>
          <v-card>
            <v-card-title
              class="headline grey lighten-2"
              primary-title>
              Table Universe
              <v-spacer></v-spacer>
              <v-icon @click="dialog = false">close</v-icon>
            </v-card-title>
            <v-card-text>
              <v-text-field v-model="selectedTableUniverse"></v-text-field>
            </v-card-text>
            <v-divider></v-divider>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="primary"
                flat @click="dialog = false">okay
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-btn
          slot="activator"
          color="grey lighten-2"
          dark
          icon
          @click="clearTableUniverse()">
          <v-icon>close</v-icon>
        </v-btn>
      </v-flex>
      <v-flex xs2>
        <v-subheader>Presentational Style:</v-subheader>
      </v-flex>
      <v-flex xs10>
        <v-select
          :items="programs"
          label="Select a Presentational Style"></v-select>
      </v-flex>
      <v-flex xs12 text-right>
        <v-btn :disabled="!valid" @click="submit">Save</v-btn>
      </v-flex>
    </v-layout>
  </v-form>
</template>
<script>
import { mapActions } from 'vuex'
export default {
  data () {
    return {
      tableUniverse: [],
      dialog: false,
      valid: true,
      selectedProgramRules: [
        v => !!v || 'Please select a program'
      ],
      selectedComponentRules: [
        v => !!v || 'Please select a component'
      ],
      selectedDatasetRules: [
        v => !!v || 'Please select a dataset'
      ],
      selectedTableIdRules: [
        v => !!v || 'Please enter a tableId'
      ],
      selectedDisplayLabelRules: [
        v => !!v || 'Please enter a display label'
      ]
    }
  },
  /* gets the list of programs associated with the user */
  created () {
    console.log('dispatching the actions')
  },
  methods: {
    ...mapActions({
    /* dispatches an action to get the list of components as soon as the user selects a program */
      getComponentsList: function () {
        console.log('executing something after selecting the program before the component is updated')
        this.$store.dispatch('getComponents')
      },
      /* dispatches an action to get the list of datasets as soon as the user selects a program and component */
      getDatasetsList: function () {
        console.log('executing something after selecting the component type before the component is updated')
        this.$store.dispatch('getDatasets')
      },
      clearTableUniverse: function () {
        console.log('dispatching table universe action')
        this.$store.dispatch('updateSelectedTableUniverse', [])
      }
    }),
    submit () {
      alert('cretaed a table')
    }
  },
  computed: {
  /* gets the list of programs associated to the user and displays in the template whose v-model is programs */
    programs () {
      console.log('getting the list of programs')
      return this.$store.state.programs
    },
    /* gets the list of components associated with the program and displays in the template whose v-model is components */
    components () {
      console.log('getting the list of components list')
      return this.$store.state.components
    },
    /* gets the list of datasets associated with the program and components and displays in the template whose v-model is datasets */
    datasets () {
      console.log('getting the list of datasets list')
      return this.$store.state.datasets
    },
    selectedProgram: {
    /* gets the selected program by the user from the store, usually executes when a user is editing the table. For new table anyway it will be null/empty */
      get () {
        console.log(this.$store.state.selectedProgram)
        return this.$store.state.selectedProgram
      },
      /* sets the user selected program from the vue template binding and dispatches an action to commit it to the store, will be used to get the list of components associated with the program */
      set (selectedProgram) {
        console.log(selectedProgram)
        this.$store.dispatch('updateSelectedProgram', selectedProgram)
      }
    },
    selectedComponent: {
    /* gets the selected component by the user from the store, usually executes when a user is editing the table. For new table anyway it will be null/empty */
      get () {
        console.log(this.$store.state.selectedComponent)
        return this.$store.state.selectedComponent
      },
      /* sets the user selected component from the vue template binding and dispatches an action to commit it to the store, will be used to get the list of datasets associated with the program and component */
      set (selectedComponent) {
        console.log(selectedComponent)
        this.$store.dispatch('updateSelectedComponent', selectedComponent)
      }
    },
    selectedTableUniverse: {
      get () {
        console.log(this.$store.state.selectedTableUniverse)
        return this.$store.state.selectedTableUniverse
      },
      set (selectedTableUniverse) {
        console.log(selectedTableUniverse)
        this.$store.dispatch('updateSelectedTableUniverse', selectedTableUniverse)
      }
    },
    selectedDataset: {
    /* gets the selected dataset by the user from the store, usually executes when a user is editing the table. For new table anyway it will be null/empty */
      get () {
        console.log(this.$store.state.selectedDataset)
        return this.$store.state.selectedDataset
      },
      /* sets the user selected component from the vue template binding and dispatches an action to commit it to the store, will be used to get the list of datasets associated with the program and component */
      set (selectedDataset) {
        console.log(selectedDataset)
        this.$store.dispatch('updateSelectedDataset', selectedDataset)
      }
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit css to this component only -->
<style scoped>
  span {
    color: #ff0000
  }
</style>
