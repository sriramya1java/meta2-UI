<template>
<div>
  <div class="main">
    <v-layout row wrap class="pt-4" align-right>
      <v-btn v-on:click="expandAll" dark>Expand</v-btn>
      <v-btn v-on:click="collapseAll" dark>Collapse</v-btn>
    </v-layout>
    <v-radio-group row :mandatory="true" v-model="showOnCheck">
      <v-radio label="Category Label" value="label"></v-radio>
      <v-radio label="Category Id" value="id"></v-radio>
    </v-radio-group>
    <div class="container" style="width: 100%">
      <div class="treeSelf">
        <vue-drag-tree :data='categoriesList' :allowDrag='allowDrag' ontoggle="true" :allowDrop='allowDropLeft' :showWhat='showOnCheck' :fromWhere='left' :defaultText='"New Node"' @current-node-clicked='curNodeClicked' @drag="dragHandler" @drag-enter="dragEnterHandler" @drag-leave="dragLeaveHandler" @drag-over="dragOverHandler" @drag-end="dragEndHandler" @drop="dropHandler"></vue-drag-tree>
      </div>
      <div class="treeSelf">
        <vue-drag-tree :data='categoriesList1' :allowDrag='allowDrag' :auto-expand="autoExpand" ontoggle="true" :allowDrop='allowDropRight' :showWhat='showOnCheck' :fromWhere='right' :defaultText='"New Node"' @current-node-clicked='curNodeClicked' @drag="dragHandler" @drag-enter="dragEnterHandler" @drag-leave="dragLeaveHandler" @drag-over="dragOverHandler" @drag-end="dragEndHandler" @drop="dropHandler"></vue-drag-tree>
      </div>
      <div class='showSec'>
        <pre>{{formatData1}}</pre>
      </div>
    </div>
    <v-layout row wrap class="pt-4" align-right>
      <v-btn slot="activator" dark>Save</v-btn>
      <v-btn slot="activator" dark>Discard</v-btn>
    </v-layout>
    <!-- <div class=row>
    <div class='showSec'>
        <pre>{{formatData}}</pre>
      </div>
      <div class='showSec'>
        <pre>{{formatData1}}</pre>
      </div>
      </div> -->
  </div>
</div>
</template>
<script>
import VueDragTree from './VueDragTree.vue'
import ContextMenu from './Context-menu.vue'
export default{
  props: ['axesDimensionsSelected'],
  components: {
    VueDragTree,
    ContextMenu
  },
  data () {
    return {
      en: false,
      dialog: false,
      left: 'left',
      right: 'right',
      autoExpand: true,
      showOnCheck: 'label',
      selectedItem: '',
      contextMenuWidth: null,
      contextMenuHeight: null
    }
  },
  computed: {
    categoriesList1: {
      get () {
        return this.$store.state.categoriesList1
      }
    },
    categoriesList: {
      get () {
        this.$store.state.categoriesList.forEach(x => {
          x.children = []
        })
        console.log(this.$store.state.categoriesList)

        // to delete children and key from the list
        /* this.$store.state.categoriesList.forEach(x => {
          delete x.children
          delete x.key
        }) */

        return this.$store.state.categoriesList
      }
    },
    verticalDimensionsSelected () {
      return this.$store.state.verticalDimensionsSelected
    },
    horizontalDimensionsSelected () {
      return this.$store.state.horizontalDimensionsSelected
    },
    outsideDimensionsSelected () {
      return this.$store.state.outsideDimensionsSelected
    },
    formatData () {
      return JSON.stringify(this.categoriesList, null, 2)
    },
    formatData1 () {
      return JSON.stringify(this.categoriesList1, null, 2)
    }
  },
  created: function () {
    console.log('categories component get created-----------------')
    /* console.log(this.axesDimensionsSelected) */
  },
  methods: {
    allowDrag (model, component) {
      return true
    },
    allowDropLeft (model, component) {
      return false
    },
    allowDropRight (model, component) {
      return true
    },
    curNodeClicked (model, component) {
      // console.log('******************************', this.curNodeClicked)
      console.log(model.label)
      // model.label = 'Test'
    },
    dragHandler (model, component, e) {
      // console.log('dragHandler: ', model, component, e)
    },
    dragEnterHandler (model, component, e) {
      // console.log('dragEnterHandler: ', model, component, e)
    },
    dragLeaveHandler (model, component, e) {
      // console.log('dragLeaveHandler: ', model, component, e)
    },
    dragOverHandler (model, component, e) {
      // console.log('dragOverHandler: ', model, component, e)
    },
    dragEndHandler (model, component, e) {
      // console.log('dragEndHandler: ', model, component, e)
    },
    dropHandler (model, component, e) {
      // console.log('dropHandler: ', model, component, e)
      const list = model.children
      // change the key
      if (list && list !== undefined) {
        list.forEach(x => {
          let lastRandom
          let random
          if (lastRandom === undefined) {
            random = Math.floor(Math.random() * (1000000000000 + 1))
          } else {
            random = Math.floor(Math.random() * (1000000000000))
            if (random >= lastRandom) random += 1
          }
          lastRandom = random
          x.key = lastRandom
        })
      }
    },
    collapseAll () {
      this.autoExpand = false
    },
    expandAll () {
      this.autoExpand = true
    },
    showContextMenu: () => {
      let menu = document.getElementById('context-menu')
      if (!this.contextMenuWidth || !this.contextMenuHeight) {
        menu.style.visibility = 'hidden'
        menu.style.display = 'block'
        this.contextMenuWidth = menu.offsetWidth
        this.contextMenuHeight = menu.offsetHeight
        menu.removeAttribute('style')
        console.log(menu)
      }

      if ((this.contextMenuWidth + event.pageX) >= window.innerWidth) {
        menu.style.left = (event.pageX - this.contextMenuWidth) + 'px'
      } else {
        menu.style.left = event.pageX + 'px'
      }

      if ((this.contextMenuHeight + event.pageY) >= window.innerHeight) {
        menu.style.top = (event.pageY - this.contextMenuHeight) + 'px'
      } else {
        menu.style.top = event.pageY + 'px'
      }
      menu.classList.add('active')
    },
    hideContextMenu: () => {
      document.getElementById('context-menu').classList.remove('active')
    }
  },
  watch: {
    showOnCheck (val) {
      console.log(val)
      this.showOnCheck = val
    }
  }
}
</script>

<style scoped>
.main {
  display: flex;
  flex-direction: column;
  font-size: 35px;
  align-items: center;
}
.sentence {
  background: #d3dce6;
  margin-top: 2rem;
  width: 50%;
  padding: 1rem;
  font-size: 25px;
}
.senMain {
  background: white;
  padding: 1rem;
  color: #8492a6;
}
.lang {
  font-size: 20px;
  color: #8492a6;
  font-weight: bold;
}
ul {
  margin-left: 1rem;
}
.container {
  width: 60%;
  display: flex;
  margin-top: 2rem;
  justify-content: space-between;
  padding: 0 5rem;
}
.treeSelf {
  border: 3px solid #e5e9f2;
  color: white;
  width: 40%;
  text-align: left;
  padding: 1rem;
  height: 300px;
  overflow: scroll;
}
.showSec {
  border: 3px solid #e5e9f2;
  width: 40%;
  text-align: left;
  padding: 1rem;
  font-size: 15px;
}
.howtouse {
  text-align: center;
  width: 30%;
  background: #e5e9f2;
  margin-top: 1rem;
  padding: 1rem;
  font-size: 50px;
  font-weight: bold;
}
.howtouse > div {
  font-family: 'Tangerine';
  font-size: 5rem;
}
.getstart > div {
  background: #d3dce6;
  padding: 0.5rem 1rem;
}
.getstart > p {
  margin: 0.5rem 0;
}
a {
  color: black;
}
.tree-view-item-key {
  font-size: 20px;
  color: black;
}
.tree-view-item-value {
  font-size: 20px;
  font-weight: bold;
  color: white;
}
</style>
