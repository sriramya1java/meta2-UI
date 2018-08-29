<template>
<v-container fluid>
<v-alert :value="showalert">Please select atleast one option!!</v-alert>
 <v-layout row wrap align-center>
        <v-flex xs5>
			<v-subheader>Dimensions:</v-subheader>
          <select id="dimensionsList" v-model="dimensionsSelected" multiple="multiple" style="min-height: 600px;background:#f8f8f8; min-width:400px;">
            <option v-for="optionLeft in dimensionsList" v-bind:value="optionLeft" >{{ optionLeft.label }}</option>
          </select>
         </v-flex>
         <v-flex xs7>
           <v-layout row wrap align-center>
             <v-flex xs2>
             <v-btn slot="activator" color="grey" dark icon v-on:click="multiselect_rightSelected(dimensionsSelected, dimensionsList, verticalDimensionsList, 'vertical')">
                <v-icon>chevron_right</v-icon>
              </v-btn><br>
              <v-btn slot="activator" color="grey" dark icon v-on:click="multiselect_leftSelected(verticalDimensionsSelected, verticalDimensionsList, dimensionsList, 'vertical')">
                <v-icon>chevron_left</v-icon>
              </v-btn>
             </v-flex>
             <v-flex xs6>
             <v-subheader>vertical axis:</v-subheader>
                <select id="verticalDimensionsList" class="form-control" v-model="verticalDimensionsSelected"  multiple="multiple" style="min-height: 150px;background:#f8f8f8; min-width:400px;">
                  <option v-for="optionRight in verticalDimensionsList"  v-bind:value="optionRight">{{ optionRight.label }}</option>
                </select>
             </v-flex>
             <v-flex xs4>
              <v-btn slot="activator" color="grey" dark icon v-on:click="moveVerticalAxisElement(-1)">
                <v-icon>expand_less</v-icon>
              </v-btn><br>
              <v-btn slot="activator" color="grey" dark icon v-on:click="moveVerticalAxisElement(1)">
                <v-icon>expand_more</v-icon>
              </v-btn><br>
              <v-dialog v-model=dialog persistent width="1000" :disabled="categoriesDialog()">
                <v-btn slot="activator" color="grey" dark icon @click="categoriesApiCall('vertical')">
                <v-icon>edit</v-icon>
              </v-btn>
              <v-card>
                <v-card-title class="headline grey" primary-title dense>
                   Categories
                   <v-spacer></v-spacer>
                   <v-btn slot="activator" icon @click.native="dialog = false">
                <v-icon>close</v-icon>
              </v-btn>
                </v-card-title>
                <v-card-text>
                   <v-subheader align-center>Drag and Drop categories to rearrange them. Right click a category for more options.</v-subheader>
                   <categories></categories>
                </v-card-text>
              </v-card>
              </v-dialog>
             </v-flex>
           </v-layout>
           <v-layout row wrap align-center>
           <v-flex xs2>
             <v-btn slot="activator" color="grey" dark icon v-on:click="multiselect_rightSelected(dimensionsSelected, dimensionsList, horizontalDimensionsList, 'horizontal')">
                <v-icon>chevron_right</v-icon>
              </v-btn><br>
              <v-btn slot="activator" color="grey" dark icon v-on:click="multiselect_leftSelected(horizontalDimensionsSelected, horizontalDimensionsList, dimensionsList, 'horizontal')">
                <v-icon>chevron_left</v-icon>
              </v-btn>
             </v-flex>
             <v-flex xs6>
            <v-subheader>Horizontal axis:</v-subheader>
                <select id="horizontalDimensionsList" class="form-control" v-model="horizontalDimensionsSelected"  multiple="multiple" style="min-height: 150px;background:#f8f8f8; min-width:400px;">
                  <option v-for="optionRight in horizontalDimensionsList" v-bind:value="optionRight" v-model="horizontalDimensionsList">{{ optionRight.label }}</option>
                </select>
             </v-flex>
             <v-flex xs4>
              <v-btn slot="activator" color="grey" dark icon v-on:click="moveHorizontalAxisElement(-1)">
                <v-icon>expand_less</v-icon>
              </v-btn><br>
              <v-btn slot="activator" color="grey" dark icon v-on:click="moveHorizontalAxisElement(1)">
                <v-icon>expand_more</v-icon>
              </v-btn><br>
              <v-dialog v-model=dialog persistent width="1000">
                <v-btn slot="activator"color="grey" dark icon @click="categoriesApiCall('horizontal')">
                <v-icon>edit</v-icon>
              </v-btn>
              <v-card>
                <v-card-title class="headline grey" primary-title dense>
                   Categories
                   <v-spacer></v-spacer>
                   <v-btn slot="activator" icon @click.native="dialog = false">
                <v-icon>close</v-icon>
              </v-btn>
                </v-card-title>
                <v-card-text>
                   <v-subheader align-center>Drag and Drop categories to rearrange them. Right click a category for more options.</v-subheader>
                   <categories></categories>
                </v-card-text>
              </v-card>
              </v-dialog>
             </v-flex>
           </v-layout>
           <v-layout row wrap align-center>
           <v-flex xs2>
             <v-btn slot="activator" color="grey" dark icon v-on:click="multiselect_rightSelected(dimensionsSelected, dimensionsList, outsideDimensionsList, 'outside')">
                <v-icon>chevron_right</v-icon>
              </v-btn><br>
              <v-btn slot="activator" color="grey" dark icon v-on:click="multiselect_leftSelected(outsideDimensionsSelected, outsideDimensionsList, dimensionsList, 'outside')">
                <v-icon>chevron_left</v-icon>
              </v-btn>
             </v-flex>
             <v-flex xs6>
             <v-subheader>outside of the table:</v-subheader>
                <select id="outsideDimensionsList" class="form-control" v-model="outsideDimensionsSelected"  multiple="multiple" style="min-height: 150px;background:#f8f8f8; min-width:400px;">
                  <option v-for="optionRight in outsideDimensionsList" v-bind:value="optionRight" v-model="outsideDimensionsList">{{ optionRight.label }}</option>
                </select>
             </v-flex>
             <v-flex xs4>
              <v-btn slot="activator" color="grey" dark icon v-on:click="moveOutsideAxisElement(-1)">
                <v-icon>expand_less</v-icon>
              </v-btn><br>
              <v-btn slot="activator" color="grey" dark icon v-on:click="moveOutsideAxisElement(1)">
                <v-icon>expand_more</v-icon>
              </v-btn><br>
              <v-dialog v-model=dialog persistent width="1000">
                <v-btn slot="activator" color="grey" dark icon @click="categoriesApiCall('outside')">
                <v-icon>edit</v-icon>
              </v-btn>
              <v-card>
                <v-card-title class="headline grey" primary-title dense>
                   Categories
                   <v-spacer></v-spacer>
                   <v-btn slot="activator" icon @click.native="dialog = false">
                <v-icon>close</v-icon>
              </v-btn>
                </v-card-title>
                <v-card-text>
                   <v-subheader align-center>Drag and Drop categories to rearrange them. Right click a category for more options.</v-subheader>
                   <categories></categories>
                </v-card-text>
              </v-card>
              </v-dialog>
             </v-flex>
           </v-layout>
         </v-flex>
     </v-layout>
   </v-container>
</template>
<script>
import Categories from '@/components/Tables/categories/Categories'
export default {
  name: 'Dimensions',
  components: {
    Categories
  },
  data () {
    return {
      showalert: false,
      /* verticalDimensionsSelected: [], */
      /* horizontalDimensionsSelected: [], */
      /* outsideDimensionsSelected: [], */
      dimensionsSelected: [],
      dialog: false
    }
  },
  computed: {
    dimensionsList () {
      console.log('------------gets the list of dimensions')
      console.log(this.$store.state.dimensionsList)
      return this.$store.state.dimensionsList
    },
    verticalDimensionsList: {
      get () {
        return this.$store.state.verticalDimensionsList
      },
      set (verticalDimensionsList) {
        console.log('----------------------vertical dimensions list-----------', verticalDimensionsList)
        this.$store.dispatch('updateVerticalDimensionsList', verticalDimensionsList)
      }
    },
    horizontalDimensionsList () {
      console.log('------------gets the list of  horizontal dimensions')
      console.log(this.$store.state.horizontalDimensionsList)
      return this.$store.state.horizontalDimensionsList
    },
    outsideDimensionsList () {
      console.log('------------gets the list of  OUTSIDE dimensions')
      console.log(this.$store.state.outsideDimensionsList)
      return this.$store.state.outsideDimensionsList
    },
    verticalDimensionsSelected: {
      get () {
        return this.$store.state.verticalDimensionsSelected
      },
      set (verticalDimensionsSelected) {
        if (verticalDimensionsSelected !== undefined && verticalDimensionsSelected !== null && verticalDimensionsSelected.length > 0) {
          console.log('----------------------vertical dimensions selected-----------', verticalDimensionsSelected)
          this.$store.dispatch('verticalDimensionSelectedUpdated', verticalDimensionsSelected)
        }
      }
    },
    horizontalDimensionsSelected: {
      get () {
        return this.$store.state.horizontalDimensionsSelected
      },
      set (horizontalDimensionsSelected) {
        if (horizontalDimensionsSelected !== undefined && horizontalDimensionsSelected !== null && horizontalDimensionsSelected.length > 0) {
          console.log('************************ horizontal dimensions selected **********************', horizontalDimensionsSelected)
          this.$store.dispatch('horizontalDimensionSelectedUpdated', horizontalDimensionsSelected)
        }
      }
    },
    outsideDimensionsSelected: {
      get () {
        return this.$store.state.outsideDimensionsSelected
      },
      set (outsideDimensionsSelected) {
        if (outsideDimensionsSelected !== undefined && outsideDimensionsSelected !== null && outsideDimensionsSelected.length > 0) {
          console.log('########################## OUTSIDE dimensions selected ####################', outsideDimensionsSelected)
          this.$store.dispatch('outsideDimensionSelectedUpdated', outsideDimensionsSelected)
        }
      }
    }
  },
  methods: {
    multiselect_rightSelected: function (dimensionsSelected, dimensionsList, axesDimensionsList, axes) {
      this.showalert = dimensionsSelected.length === 0
      dimensionsSelected.forEach(x => {
        const leftIndex = dimensionsList.findIndex(y => y === x)
        dimensionsList.splice(leftIndex, 1)
        const rightIndex = axesDimensionsList.findIndex(z => z === x)
        if (!(rightIndex > -1)) {
          axesDimensionsList.push(x)
        }
      })
      if (axes === 'vertical') {
        this.$store.dispatch('updateVerticalDimensionsList', axesDimensionsList)
      } else if (axes === 'horizontal') {
        this.$store.dispatch('updateHorizontalDimensionsList', axesDimensionsList)
      } else if (axes === 'outside') {
        this.$store.dispatch('updateOutsideDimensionsList', axesDimensionsList)
      }
    },
    multiselect_leftSelected: function (axesDimensionsSelected, axesDimensionsList, dimensionsList, axes) {
      this.showalert = axesDimensionsSelected.length === 0
      axesDimensionsSelected.forEach(x => {
        const rightIndex = axesDimensionsList.findIndex(y => y === x)
        axesDimensionsList.splice(rightIndex, 1)
        const leftIndex = dimensionsList.findIndex(z => z === x)
        if (!(leftIndex > -1)) {
          dimensionsList.push(x)
        }
      })
      if (axes === 'vertical') {
        this.$store.dispatch('updateVerticalDimensionsList', axesDimensionsList)
        this.$store.commit('SET_DIMENSIONS', dimensionsList)
      } else if (axes === 'horizontal') {
        this.$store.dispatch('updateHorizontalDimensionsList', axesDimensionsList)
      } else if (axes === 'outside') {
        this.$store.dispatch('updateOutsideDimensionsList', axesDimensionsList)
      }
    },
    categoriesDialog: function () {
      if (this.outsideDimensionsSelected.length === 1 || this.horizontalDimensionsSelected.length === 1 || this.verticalDimensionsSelected.length === 1) {
        return false
      } else {
        return true
      }
    },
    categoriesApiCall: function (axes) {
      console.log('---------------calling categories API-----------', axes)
      if (this.outsideDimensionsSelected.length === 1 || this.horizontalDimensionsSelected.length === 1 || this.verticalDimensionsSelected.length === 1) {
        this.$store.dispatch('getCategoriesList', axes)
      } else {
        alert('choose atleast one and only one dimension')
      }
    }
    /* moveVerticalAxisElement: function (positionChange) {
      let oldIndex = verticalDimensionsList.findIndex(y => y.label === this.verticalDimensionsSelected[0].label)
      if (oldIndex > -1) {
        let newIndex = (oldIndex + positionChange)
        if (newIndex < 0) {
          newIndex = 0
        } else if (newIndex >= verticalDimensionsList.length) {
          newIndex = verticalDimensionsList.length
        }
        let arrayClone = verticalDimensionsList.slice()
        arrayClone.splice(oldIndex, 1)
        arrayClone.splice(newIndex, 0, this.verticalDimensionsSelected[0])
        verticalDimensionsList = arrayClone
        return verticalDimensionsList
      }
      return verticalDimensionsList
    },
    moveHorizontalAxisElement: function (positionChange) {
      let oldIndex = this.horizontalDimensionsList.findIndex(y => y.label === this.horizontalDimensionsSelected[0].label)
      if (oldIndex > -1) {
        let newIndex = (oldIndex + positionChange)
        if (newIndex < 0) {
          newIndex = 0
        } else if (newIndex >= this.horizontalDimensionsList.length) {
          newIndex = this.horizontalDimensionsList.length
        }
        let arrayClone = this.horizontalDimensionsList.slice()
        arrayClone.splice(oldIndex, 1)
        arrayClone.splice(newIndex, 0, this.horizontalDimensionsSelected[0])
        this.horizontalDimensionsList = arrayClone
        return this.horizontalDimensionsList
      }
      return this.horizontalDimensionsList
    },
    moveOutsideAxisElement: function (positionChange) {
      let oldIndex = this.outsideDimensionsList.findIndex(y => y.label === this.outsideDimensionsSelected[0].label)
      if (oldIndex > -1) {
        let newIndex = (oldIndex + positionChange)
        if (newIndex < 0) {
          newIndex = 0
        } else if (newIndex >= this.outsideDimensionsList.length) {
          newIndex = this.outsideDimensionsList.length
        }
        let arrayClone = this.outsideDimensionsList.slice()
        arrayClone.splice(oldIndex, 1)
        arrayClone.splice(newIndex, 0, this.outsideDimensionsSelected[0])
        this.outsideDimensionsList = arrayClone
        return this.outsideDimensionsList
      }
      return this.outsideDimensionsList
    } */
  }
}
</script>
<style scoped>
  .list-vertical {
    height: 140px;
    overflow-y: scroll;
    margin: 0;
    min-height: 140px;
  }
  .list-vertical li {
    margin-bottom: 6px;
  }
</style>